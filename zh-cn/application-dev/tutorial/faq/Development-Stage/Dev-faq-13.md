# ArkUI-X 如何实现结束输入会话的能力

HarmonyOS NEXT上inputMethodController.stopInputSession用于结束输入会话，该能力当前不支持跨平台

**【替代方案】**可以用focusControl.requestFocus把焦点转移到其他一个不涉及输入法的组件上，以此关闭输入法，示例代码如下：

```java
 TextInput()
   .margin(20)

Button('stopInputSession')
   .width('40%')
   .height(50)
   .id('button')
   .onClick(async () => {
      focusControl.requestFocus('button')
   })

```