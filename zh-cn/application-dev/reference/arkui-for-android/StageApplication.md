## StageApplication

StageApplication是Application的子类，是Stage模型Android应用的入口。ArkUI-X Stage模型Android平台应用开发时，需要继承StageApplication。

## 方法概要

| 类型    | 方法             | 描述                                                         |
| ------- | ---------------- | ------------------------------------------------------------ |
| boolean | onShouldLoadUI() | 控制ArkUI-X框架是否加载ArkUI，如不加载，应用启动占用内存降低。 |

## 方法说明

### onShouldLoadUI

```java
/**
 * Should load UI.
 *
 * @return Return true if need load UI, else return false.
 */
public boolean onShouldLoadUI();
```

**描述：**

控制ArkUI-X框架是否加载ArkUI，如不加载，应用启动占用内存降低。<br>
禁止在函数中实现无关逻辑，开发者应保证函数返回值为确定的boolean值（true或false）。

**参数：** 

无

**返回值：** 

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 函数返回true表示需要加载ArkUI，函数返回false表示不需要加载ArkUI。 |

**示例：** 

```java
// MyStageApplication.java
public class MyStageApplication extends StageApplication {
    @Override
    public boolean onShouldLoadUI() {
        return false;
    }
}
```

