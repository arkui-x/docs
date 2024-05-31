# \@Extend Decorator: Extension of Built-in Components


Apart from\@Styles used to extend styles, AkrUI also provides \@Extend, which allows you to add a new attribute feature to a built-in component.


> **NOTE**
>
> Since API version 9, this decorator is supported in ArkTS widgets.


## Rules of Use


### Syntax


```ts
@Extend(UIComponentName) function functionName { ... }
```


### Rules of Use

- Unlike \@Styles, \@Extend can be defined only globally, that is, outside a component declaration.

- Unlike \@Styles, \@Extend can encapsulate private attributes and events of specified components and predefine \@Extend decorated methods of the same component.

  ```ts
  // @Extend(Text) supports the private attribute fontColor of the <Text> component.
  @Extend(Text) function fancy () {
    .fontColor(Color.Red)
  }
  // superFancyText can call the predefined fancy method.
  @Extend(Text) function superFancyText(size:number) {
      .fontSize(size)
      .fancy()
  }
  ```


- Unlike \@Styles, \@Extend decorated methods support parameters. You can pass parameters when calling such methods. Regular TypeScript provisions for method parameters apply.

  ```ts
  // xxx.ets
  @Extend(Text) function fancy (fontSize: number) {
    .fontColor(Color.Red)
    .fontSize(fontSize)
  }

  @Entry
  @Component
  struct FancyUse {
    build() {
      Row({ space: 10 }) {
        Text('Fancy')
          .fancy(16)
        Text('Fancy')
          .fancy(24)
      }
    }
  }
  ```

- A function can be passed as a parameter in an \@Extend decorated method to be used as the handler of the event.

  ```ts
  @Extend(Text) function makeMeClick(onClick: () => void) {
    .backgroundColor(Color.Blue)
    .onClick(onClick)
  }

  @Entry
  @Component
  struct FancyUse {
    @State label: string = 'Hello World';

    onClickHandler() {
      this.label = 'Hello ArkUI';
    }

    build() {
      Row({ space: 10 }) {
        Text(`${this.label}`)
          .makeMeClick(this.onClickHandler.bind(this))
      }
    }
  }
  ```

- A [state variable](arkts-state-management-overview.md) can be passed as a parameter in an \@Extend decorated method. When the state variable changes, the UI is updated and re-rendered.

  ```ts
  @Extend(Text) function fancy (fontSize: number) {
    .fontColor(Color.Red)
    .fontSize(fontSize)
  }

  @Entry
  @Component
  struct FancyUse {
    @State fontSizeValue: number = 20
    build() {
      Row({ space: 10 }) {
        Text('Fancy')
          .fancy(this.fontSizeValue)
          .onClick(() => {
            this.fontSizeValue = 30
          })
      }
    }
  }
  ```


## Application Scenarios

The following example declares three **\<Text>** components. The **fontStyle**, **fontWeight**, and **backgroundColor** styles are set for each **\<Text>** component.


```ts
@Entry
@Component
struct FancyUse {
  @State label: string = 'Hello World'

  build() {
    Row({ space: 10 }) {
      Text(`${this.label}`)
        .fontStyle(FontStyle.Italic)
        .fontWeight(100)
        .backgroundColor(Color.Blue)
      Text(`${this.label}`)
        .fontStyle(FontStyle.Italic)
        .fontWeight(200)
        .backgroundColor(Color.Pink)
      Text(`${this.label}`)
        .fontStyle(FontStyle.Italic)
        .fontWeight(300)
        .backgroundColor(Color.Orange)
    }.margin('20%')
  }
}
```

\@Extend combines and reuses styles. The following is an example:


```ts
@Extend(Text) function fancyText(weightValue: number, color: Color) {
  .fontStyle(FontStyle.Italic)
  .fontWeight(weightValue)
  .backgroundColor(color)
}
```

With the use of \@Extend, the code readability is enhanced.


```ts
@Entry
@Component
struct FancyUse {
  @State label: string = 'Hello World'

  build() {
    Row({ space: 10 }) {
      Text(`${this.label}`)
        .fancyText(100, Color.Blue)
      Text(`${this.label}`)
        .fancyText(200, Color.Pink)
      Text(`${this.label}`)
        .fancyText(300, Color.Orange)
    }.margin('20%')
  }
}
```
