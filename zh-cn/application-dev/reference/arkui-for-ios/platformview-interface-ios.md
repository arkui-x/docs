# PlatformView (平台视图 iOS)

本模块提供平台视图的iOS平台的接口，包括接口、数据类型等。下面进行详细的介绍说明。

> **说明：**
>
> 本模块接口从API version 12 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## PlatformView接口


## view

获得原生组件View。

(UIView*)view

**参数：** 

无

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| UIView* | 原生组件View。 |


## onDispose

资源销毁的处置。

(void)onDispose

**参数：** 

无

**返回值：** 

无

## getPlatformViewID

返回原生组件的ID。

(NSString*)getPlatformViewID

**参数：** 

无

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| NSString* | 返回原生组件的ID。 |

**示例：**

```Objective-C
//实现 IPlatformView 接口，重写 getView 方法返回 webView 对象。

@interface MyWebview : NSObject<IPlatformView>
- (UIView *) view;
- (void) onDispose;
- (NSString *) getPlatformViewID;
- (instancetype)initWithFrame:(NSString*) data;
@end

@implementation MyWebview {
    WKWebView *webView;
    NSString* viewTag;
}

- (instancetype)initWithFrame:(NSString*) data
{
    self = [super init];
    if (self) {
        viewTag = @"WebView";
        webView = [[WKWebView alloc] init];
        ``````
    }
    return self;
}

- (UIView*) view {
    return webView;
}

- (void)onDispose {
    webView = nil;
}

- (NSString*) getPlatformViewID {
    return viewTag;
}
@end
```

## PlatformViewFactory接口


## getPlatformView

获得IPlatformView接口。

(NSObject<IPlatformView>*) getPlatformView:(NSString*) platformViewId


**参数：** 

| 参数名          | 类型             | 必填 | 说明           |
| --------------- | ---------------- | ---- | -------------- |
| platformViewId | NSString* | 是   | 原生组件的ID。 |

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| NSObject<IPlatformView>* | IPlatformView接口。 |

(NSObject<IPlatformView>*) getPlatformView:(NSString*) platformViewId data:(NSString*) data<sup>22+</sup>
> **说明：**
>
> 可选重载，用于需要从ArkTs侧传递数据到原生侧的场景。

**参数：** 

| 参数名          | 类型             | 必填 | 说明           |
| --------------- | ---------------- | ---- | -------------- |
| platformViewId | NSString* | 是   | 原生组件的ID。 |
| data | NSString* | 是   | ArkTs侧传递的数据。 |

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| NSObject<IPlatformView>* | IPlatformView接口。 |

**示例：**

```Objective-C
//实现 PlatformViewFactory 接口，重写 getPlatformView 方法返回 IPlatformView 对象。

@interface MyPlatformViewFactory : NSObject<PlatformViewFactory>

- (NSObject<IPlatformView>*) getPlatformView:(NSString*) platformViewId;
- (NSObject<IPlatformView>*) getPlatformView:(NSString*) platformViewId data:(NSString*) data;
- initWithFrame;
@end

@implementation MyPlatformViewFactory {

}

- (instancetype)initWithFrame{
    return self;
}

- (NSObject<IPlatformView>*) getPlatformView:(NSString*) platformViewId {
    NSLog(@"[PlatformViewUI] getPlatfformView");
    if ([platformViewId isEqualToString:@"WebView"]) {
        NSObject<IPlatformView> * view = [[MyWebview alloc] initWithFrame:nil];
        return view;
    }
    return nil;
}

- (NSObject<IPlatformView>*) getPlatformView:(NSString*) platformViewId data:(NSString*) data {
    if ([platformViewId isEqualToString:@"WebView"]) {
        NSObject<IPlatformView> * view = [[MyWebview alloc] initWithFrame:data];
        return view;
    }
    return nil;
}

```


## 完整示例

[完整示例](../../tutorial/how-to-use-platformview-on-ios.md)