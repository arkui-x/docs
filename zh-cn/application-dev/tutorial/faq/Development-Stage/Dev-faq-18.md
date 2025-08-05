# ArkUI-X 解决平台视图从ArkTS侧向原生侧传值

### 【问题现象】

ArkTS侧在构造平台视图时无法向原生侧传递参数。

### 【解决方法】

1. 在平台视图功能开发时，有需给原生侧传递参数的场景，通过PlatformView构造函数的platformViewID参数来传递。可以将ViewID、参数格式化为JSON字符串，并在原生侧平台视图工厂类方法getPlatformView中解析获取。

2. 在原生侧可增加结构体存放前端传过来的参数。

#### ArkTS侧示例代码：

```javascript
class JsonRecord {
  id: string = '';
  data: string = '';
}

@State jsonData: JsonRecord = { id: "MapView", data: JSON.stringify({ key: "liming", value: "23" }) };
@State platformViewParam : string = JSON.stringify(this.jsonData);

// 创建平台视图
PlatformView(this.platformViewParam).height('100%').width('100%')
```

#### Android侧示例代码：

1. 构建结构体及转换类

```java
//JsonRecordBean.java
public class JsonRecordBean {
    public String id;
    public String data;
}

使用下面转换方法需要在app下的build.gradle的dependencies下配置：implementation 'com.google.code.gson:gson:2.10.1'
//StringToBean.java
import com.google.gson.Gson;
public class StringToBean {
    public static <T> T convert(String json, Class<T> clazz) {
        Gson gson = new Gson();
        return gson.fromJson(json, clazz);
    }
}
```

2. 修改平台视图显示类的构造函数

```java
//MyMapView.java
private String strParameter;//用于存放ArkTS传递过来的数据
public MyMapView(Context context, Bundle savedInstanceState, String strParameter) {//在构造函数中增加参数
    mapView = new TextureMapView(context);
    mapView.onCreate(savedInstanceState);
    this.strParameter = strParameter;
}
```

3. 修改继承平台视图工厂类

```java
//MyPlatformViewFactory.java
private JsonRecordBean jsonRecordBean;//定义结构体
@Override
public IPlatformView getPlatformView(String id) {
    jsonRecordBean = StringToBean.convert(id, JsonRecordBean.class);//解析传过来的参数
    if ("MapView".equals(jsonRecordBean.id)) {
        return new MyMapView(this.context, this.savedInstanceState, jsonRecordBean.data);//将参数传入构造函数
    }
    return null;
}
```

**iOS侧示例代码：**

1.添加平台视图参数类

```objc
// PlatformViewParameter.h
@interface JsonRecordBean : NSObject

@property (nonatomic, copy) NSDictionary *data;

- (instancetype)initWithParameter:(NSString *)parameter;

@end
  
// PlatformViewParameter.m
@implementation JsonRecordBean

- (instancetype)initWithParameter:(NSString *)parameter {
    self = [super init];
    if (self) {
        if (parameter && parameter.length > 0) {
            self.data = [self dictionaryWithString:parameter];
        }
    }
    return self;
}

- (NSDictionary *)dictionaryWithString:(NSString *)jsonString {
    NSData *jsonData = [jsonString dataUsingEncoding:NSUTF8StringEncoding];
    NSError *error = nil;
    NSDictionary *dictionary = [NSJSONSerialization JSONObjectWithData:jsonData
                                                              options:kNilOptions
                                                                error:&error];
    if (!error) {
        return dictionary;
    }
    NSLog(@"JSON 解析错误: %@", error);
    return @{};
}

@end
```

2.修改平台视图工厂类

```objc
// MyPlatformViewFactory.m
@implementation MyPlatformViewFactory

- (NSObject<IPlatformView>*)getPlatformView:(NSString*)xComponentId {
    JsonRecordBean *parameter = [[JsonRecordBean alloc] initWithParameter:xComponentId];
    if ([parameter.data[@"id"] isEqualToString:@"MapView"]) {
        NSObject<IPlatformView> *view = [[MyMapView alloc] initWithParameter:parameter];
        return view;
    }
    return nil;
}

@end
```

3.修改平台视图类

```objc
// MyMapView.h
@interface MyMapView : NSObject<IPlatformView>

- (instancetype)initWithParameter:(JsonRecordBean *)parameter;
@property (nonatomic, strong) JsonRecordBean *parameter;
@property (nonatomic, strong) MKMapView *MapView;

@end
  
// MyMapView.m
@implementation MyMapView

- (instancetype)initWithParameter:(JsonRecordBean *)parameter {
    self = [super init];
    if (self) {
        _parameter = parameter;
        _MapView = [MKMapView new];
    }
    return self;
}

- (nonnull NSString *)getPlatformViewID { 
    return @"MapView";
}

- (void)onDispose { 
    _MapView = nil;
}

- (nonnull UIView *)view { 
    return self.MapView;
}

@end
```