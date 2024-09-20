# 平台视图开发指南

平台视图，提供了ArkUI-X与原生view进行混合显示。本文主要介绍Android平台进行平台视图开发的步骤说明，Android接口参考[PlatformView](../reference/arkui-for-android/platformview-interface-android.md)。

开发者需要做如下3个步骤。


1、【Android】 自定义PlatformView, PlatformViewFactory

    在 Android 端新建一个实现 IPlatformView 接口的类，并实现 getView 、onDispose()接口；
    在 Android 端新建一个实现 PlatformViewFactory 接口的类，并实现 getPlatformView()接口；

2、【Android】 定义StageActivity，注册上述的 PlatformViewFactory类

    再实现一个继承 StageActivity的类，在其构造函数中创建(1)的 PlatformViewFactory 对象；
    把 PlatformViewFactory 类注册到 StageActivity中。

3、【Arkui-X ets】使用PlatformView

    导入模块
    import PlatformView, { PlatformViewAttribute } from '@arkui-x.platformview'
    在 Android 端实现好了之后，就能在ArkUI端用ets来显示原生组件的视图了，如显示MapView。

## Android平台

### 场景：使用原生地图组件

1、自定义IPlatformView接口的实现类。

```java
// MyMapWrapper.java
import com.amap.api.maps.AMap;
import com.amap.api.maps.TextureMapView;
import ohos.ace.adapter.capability.platformview.IPlatformView;

public class MyMapWrapper implements IPlatformView {
    TextureMapView mMapView = null;
    AMap aMap = null;
    private String id = "PlatformViewTest1";

    MyMapWrapper(Context context, Bundle savedInstanceState) {
        // create TextureMapView
        mMapView = new TextureMapView(context) {
            @Override
            public boolean dispatchTouchEvent(MotionEvent event) {
                return super.dispatchTouchEvent(event);
            }
        };
        mMapView.onCreate(savedInstanceState);
        // init map
        aMap = mMapView.getMap();
        init();
    }

    private void init() {
        aMap.setOnMapLoadedListener(new AMap.OnMapLoadedListener() {
            @Override
            public void onMapLoaded() {
                setUp(aMap);
            }
        });
    }

    private void setUp(AMap amap) {
        UiSettings uiSettings = amap.getUiSettings();
        amap.showIndoorMap(true);
        uiSettings.setCompassEnabled(true);
        uiSettings.setScaleControlsEnabled(true);
        uiSettings.setMyLocationButtonEnabled(true);
    }

    public void onResume() {
        mMapView.onResume();
    }

    public void onSaveInstanceState(Bundle outState) {
        mMapView.onSaveInstanceState(outState);
    }

    @Override
    public View getView() {
        return mMapView;
    }
    @Override
    public  void onDispose() {
        mMapView.onDestroy();
    }
    @Override
    public String getPlatformViewID() {
        return id;
    }
}

```

2、自定义PlatformViewFactory接口的实现类。

```java
// MyPlatformViewFactory.java
import android.content.Context;
import ohos.ace.adapter.capability.platformview.IPlatformView;
import ohos.ace.adapter.capability.platformview.PlatformViewFactory;

public class MyPlatformViewFactory extends PlatformViewFactory {
    private Context context;
    private Bundle savedInstanceState;

    @Override
    public IPlatformView getPlatformView(String platformViewId) {
        IPlatformView pv = null;
        switch (platformViewId){
            case "PlatformViewTest1":
                // create PlatformView
                pv = new MyMapWrapper(context, savedInstanceState);
                break;
            default:
                break;
        }
        return pv;
    }

    public void setContext(Context context) {
        this.context = context;
    }

    public void setSavedInstanceState(Bundle savedInstanceState) {
        this.savedInstanceState = savedInstanceState;
    }
}

```

3、自定义StageActivity，把PlatformViewFactory实现类注册到StageActivity中。

```java
// EntryEntryAbilityActivity.java
import com.amap.api.maps.MapsInitializer;

import ohos.ace.adapter.capability.platformview.IPlatformView;
import ohos.ace.adapter.capability.platformview.PlatformViewFactory;
import ohos.stage.ability.adapter.StageActivity;

public class EntryEntryAbilityActivity extends StageActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        setInstanceName("com.example.mapapp:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
        
        MapsInitializer.updatePrivacyShow(this, true, true);
        MapsInitializer.updatePrivacyAgree(this, true);
        // create PlatformViewFactory
        MyPlatformViewFactory pf = new MyPlatformViewFactory();
        pf.setContext(this);
        pf.setSavedInstanceState(savedInstanceState);
        // register PlatformViewFactory
        registerPlatformViewFactory(pf);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
    }
    @Override
    protected void onResume() {
        super.onResume();
    }

    @Override
    protected void onPause() {
        super.onPause();
    }
    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
    }
}

```

4、在ArkTS中，使用PlatformView。
   编写ArkUI页面platformview.ets。

```java
// platformview.ets
import PlatformView, { PlatformViewAttribute } from '@arkui-x.platformview';
@Entry
@Component
struct PlatformView {
  @State changeValue: string = ''
  @State submitValue: string = ''
  controller: SearchController = new SearchController()
  build() {
    Column() {
      PlatformView('PlatformViewTest1')
        .width('100%')
        .height('80%')
        .backgroundColor(Color.Gray)

      Search({ value: this.changeValue, placeholder: 'Type to search...', controller: this.controller })
        .searchButton('SEARCH')
        .width('95%')
        .height(40)
        .backgroundColor('#F5F5F5')
        .placeholderColor(Color.Grey)
        .placeholderFont({ size: 14, weight: 400 })
        .textFont({ size: 14, weight: 400 })
        .onSubmit((value: string) => {
          this.submitValue = value
        })
        .onChange((value: string) => {
          this.changeValue = value
        })
        .margin(20)

    }.height('100%')
  }
}
```
