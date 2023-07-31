# Implicit Shared Element Transition

**GeometryTransition** is used for implicit shared element transitions during component switching. By specifying the in and out components through **GeometryTransition**, you can create a spatial linkage between the transition effects (such as opacity and scale) defined through the **transition** mechanism. In this way, you can guide the visual focus from the out component to the in component.

> **NOTE**

> The APIs of this module are supported since API version 10. Updates will be marked with a superscript to indicate their earliest API version.

## Attributes

| Name              | Type| Description                                                    |
| ------------------ | -------- | ------------------------------------------------------------ |
| geometryTransition | string   | ID of **geometryTransition**, which is used to set up a binding relationship. If this attribute is set to an empty string **""**, the binding relationship is cleared, and the components will not participate in the shared element transition. The value can be dynamically changed to refresh the binding relationship. One ID can be bound to only two components, which function as in and out components.|

**NOTE**

For the settings to take effect, **geometryTransition** must be used together with **animateTo**. The animation duration and curve follow the settings in **animateTo**. The **.animation** implicit animation is not supported.

## Example

```ts

// xxx.ets

@Entry

@Component

structIndex {

  @StateisShow: boolean = false


  build() {

    Stack({ alignContent:Alignment.Center }) {

      if (this.isShow) {

        Image($r('app.media.pic'))

          .autoResize(false)

          .clip(true)

          .width(300)

          .height(400)

          .offset({ y:100 })

          .geometryTransition("picture")

          .transition(TransitionEffect.OPACITY)

      } else {

        // geometryTransition is bound to a container. Therefore, a relative layout must be configured for the child components of the container.

        // The multiple levels of containers here are used to demonstrate passing of relative layout constraints.

        Column() {

          Column() {

            Image($r('app.media.icon'))

              .width('100%').height('100%')

          }.width('100%').height('100%')

        }

        .width(80)

        .height(80)

        // geometryTransition synchronizes rounded corner settings, but only for the bound component, which is the container in this example.

        // In other words, rounded corner settings of the container are synchronized, and those of the child components are not.

        .borderRadius(20)

        .clip(true)

        .geometryTransition("picture")

        // transition ensures that the component is not destructed immediately when it exits. You can customize the transition effect.

        .transition(TransitionEffect.OPACITY)

      }

    }

    .onClick(() => {

      animateTo({ duration:1000 }, () => {

        this.isShow = !this.isShow

      })

    })

  }

}

```

![geometrytransition](figures/geometrytransition.gif)
