# PlatformView (平台视图 Android)

本模块提供平台视图的Android平台的接口，包括接口、数据类型等。下面进行详细的介绍说明。

> **说明：**
>
> 本模块接口从API version 12 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## PlatformView接口


## getView

获得原生组件View。

View getView();

**参数：** 

无

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| View | 原生组件View。 |


## onDispose

资源销毁的处置。

void onDispose();

**参数：** 

无

**返回值：** 

无

## getXComponentID

返回原生组件的ID。

String getXComponentID();

**参数：** 

无

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| String | 返回原生组件的ID。 |


**示例：**

```java
//实现 IPlatformView 接口，重写 getView 方法返回 mapView 对象。

class SampleMapView implements IPlatformView {
    // 原生组件view
    private MapView mapView;
    
    public SampleMapView( int viewid, string viewtag ) {
        //...
    }
    //...
    
   @Override
    public View getView() {
        return mapView;
    }
   @Override
    public void onDispose() {}

   @Override
    public String getXComponentID(){
      return viewtag;
    }
}
```

## PlatformViewFactory接口


## getPlatformView


获得IPlatformView接口。

IPlatformView getPlatformView(String xComponentId);


**参数：** 

| 参数名          | 类型             | 必填 | 说明           |
| --------------- | ---------------- | ---- | -------------- |
| xComponentId | String | 是   | 原生组件的ID。 |

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| IPlatformView | IPlatformView接口。 |

**示例：**

```java
//实现 PlatformViewFactory 接口，重写 getPlatformView 方法返回 mapView 对象。

class SampleMapFactory implements PlatformViewFactory {
    
    String NATIVE_VIEW_TAG_ID = "oh.plugin.view.mapview";

   @Override
    public IPlatformView getPlatformView(String xComponentId) {
        // create MapView
        IPlatformView view = null;
        if(xComponentId == this.viewtag){
          view = new OH_MapView(vieweid, NATIVE_VIEW_TAG_ID);
        }
        return view;
    }

}

```


## 完整示例

[完整示例](../../tutorial/how-to-use-platformview-on-android.md)