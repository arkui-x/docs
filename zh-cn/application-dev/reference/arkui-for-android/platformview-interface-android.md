# PlatformView (平台视图 Android)

本模块提供平台视图的Android平台的接口，包括接口、数据类型等。下面进行详细的介绍说明。

> **说明：**
>
> 本模块接口从API version 12 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## PlatformView接口


## getView

获得原生组件View。

View getView()

**参数：** 

无

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| View | 原生组件View。 |


## onDispose

资源销毁的处置。

void onDispose()

**参数：** 

无

**返回值：** 

无

## getPlatformViewID

返回原生组件的ID。

String getPlatformViewID()

**参数：** 

无

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| String | 返回原生组件的ID。 |


**示例：**

```java
//实现 IPlatformView 接口，重写 getView 方法返回 webView 对象。

public class MyWebView implements IPlatformView {
    private String id = "WebView";
    private WebView mWebView;

    MyWebView(Context context) {
        this(context, "https://gitcode.com/arkui-x");
    }

    MyWebView(Context context, String url) {
        mWebView = new WebView(context);
        ```````
    }

    @Override
    public View getView() {
        return mWebView;
    }

    @Override
    public void onDispose() {
        mWebView.destroy();
    }

    @Override
    public String getPlatformViewID() {
        return id;
    }
}
```

## PlatformViewFactory接口


## getPlatformView


获得IPlatformView接口。

IPlatformView getPlatformView(String platformViewId)


**参数：** 

| 参数名          | 类型             | 必填 | 说明           |
| --------------- | ---------------- | ---- | -------------- |
| platformViewId | String | 是   | 原生组件的ID。 |

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| IPlatformView | IPlatformView接口。 |

IPlatformView getPlatformView(String platformViewId, String data)<sup>22+</sup>
> **说明：**
>
> 可选重载，用于需要从ArkTs侧传递数据到原生侧的场景。

**参数：** 

| 参数名          | 类型             | 必填 | 说明           |
| --------------- | ---------------- | ---- | -------------- |
| platformViewId | String | 是   | 原生组件的ID。 |
| data | String | 是   | ArkTs侧传递的数据。 |

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| IPlatformView | IPlatformView接口。 |

**示例：**

```java
//实现 PlatformViewFactory 接口，重写 getPlatformView 方法返回 mapView 对象。

public class MyPlatformViewFactory extends PlatformViewFactory {
    private Context context;

    @Override
    public IPlatformView getPlatformView(String Id) {
        if ("WebView".equals(Id)) {
            return new MyWebView(context);
        }
        return null;
    }

    @Override
    public IPlatformView getPlatformView(String Id, String data) {
         if ("WebView".equals(Id)) {
            return new MyWebView(context, data);
        }
        return null;
    }

    public void setContext(Context context) {
        this.context = context;
    }
}

```


## 完整示例

[完整示例](../../tutorial/how-to-use-platformview-on-android.md)