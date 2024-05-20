# PlatformView (平台视图 iOS)

本模块提供平台视图的iOS平台的接口，包括接口、数据类型等。下面进行详细的介绍说明。

> **说明：**
>
> 本模块接口从API version 12 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## PlatformView接口


## getView

获得原生组件View。

UIView* getView();

**参数：** 

无

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| UIView* | 原生组件View。 |


## onDispose

资源销毁的处置。

void onDispose();

**参数：** 

无

**返回值：** 

无

## getXComponentID

返回原生组件的ID。

NSString* getXComponentID();

**参数：** 

无

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| NSString* | 返回原生组件的ID。 |

**示例：**

```java
//实现 IPlatformView 接口，重写 getView 方法返回 mapView 对象。

@interface OH_MapView : NSObject <IPlatformView>
- (UIView*)view;
- (void) onDispose;
- (NSString*)getXComponentID;
@end

@implementation OH_MapView {
   MAMapView*_mapView;
   NSString* _viewTag;
}

- (void)initWithFrame(viewID: Int64, viewTag :NSString*)  {
  super.init();

  //  创建地图view
  _mapView= [[MAMapView alloc]initWithFrame:self.view.bounds];
  
  //....
  return ;
}

- (UIView*)view {
  return _mapView;
}

- (void)onDispose {
  return ;
}

- (NSString*)getXComponentID {
  return _viewTag;
}

@end
```

## PlatformViewFactory接口


## getPlatformView

获得IPlatformView接口。

NSObject<IPlatformView>* getPlatformView:(NSString*) xComponentId;


**参数：** 

| 参数名          | 类型             | 必填 | 说明           |
| --------------- | ---------------- | ---- | -------------- |
| xComponentId | NSString* | 是   | 原生组件的ID。 |

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| NSObject<IPlatformView>* | IPlatformView接口。 |

**示例：**

```java
//实现 PlatformViewFactory 接口，重写 getPlatformView 方法返回 IPlatformView 对象。

@interface OH_ViewFactory : NSObject <PlatformViewFactory>
 - (NSObject<IPlatformView>*) getPlatformView:(NSString*) xComponentId;
}
@end

@implementation OH_ViewFactory {
  NSObject<IPlatformView> mapView;
  Int64  _viewID;
  NSString* _viewTag;
}

- (void)initWithFrame(viewID: Int64, xComponentId :NSString*)  {
  _viewID = viewID;
  _viewTag = xComponentId;
  mapView = nil;
}

- (NSObject<IPlatformView>*)getPlatformView:(NSString*) xComponentId  {
  if(xComponentId == _viewTag){
    mapView = [[OH_MapView alloc] initWithFrame:viewid:_viewID, viewtag:xComponentId];
  }
  return mapView;
}

@end

```


## 完整示例

[完整示例](../../tutorial/how-to-use-platformview-on-ios.md)