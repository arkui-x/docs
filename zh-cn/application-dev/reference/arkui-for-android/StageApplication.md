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

### destroyVm

public static boolean destroyVm()

主动销毁ArkUI引擎的运行时及关联的Worker运行时资源。

**参数：** 无

**返回值：** 

| 类型    | 说明                                                |
| ------- | --------------------------------------------------- |
| boolean | true 表示运行时销毁成功；false 表示运行时销毁失败。 |

**返回值 false 的常见原因：**

| 原因              | 说明                                         |
| ----------------- | -------------------------------------------- |
| Runtime 已销毁    | 运行时已被销毁，无需重复操作。               |
| 存在活跃的 Worker | 当前仍有Worker线程在运行，运行时无法被销毁。 |

**示例：** 

  ```java
// OtherActivity.java Android原生业务逻辑文件

package xxx; // xxx为应用包名

import android.app.Activity;
import android.os.Bundle;
import android.util.Log;

import ohos.stage.ability.adapter.StageApplication;

public class OtherActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    @Override
    protected void onResume() {
        super.onResume();
        // 从ArkUI页面返回时，主动销毁运行时，释放资源。
        boolean result = StageApplication.destroyVm();
        if (!result) {
            // 销毁失败，可记录日志或稍后重试。
            Log.e("OtherActivity", "destroyVm failed, ArkTS runtime may still have active workers.");
        }
    }
}
  ```

