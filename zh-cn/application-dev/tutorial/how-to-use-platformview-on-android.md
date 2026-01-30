# Android平台视图开发指南

## 概述

平台视图（PlatformView）是ArkUI-X框架提供的核心能力之一，允许开发者在ArkUI界面中嵌入原生Android组件。<br>通过此能力，您可以充分利用Android 平台特有的UI组件（如TextureMapView、WebView、MediaPlayer等），实现ArkUI组件与原生视图的无缝混合渲染。<br>ArkUI侧具体API请参考[PlatformView API](../reference/apis/js-apis-PlatformView.md)，Android侧参考[PlatformView](../reference/arkui-for-android/platformview-interface-android.md)。

## 示例

### 自定义IPlatformView接口的实现类, 封装WebView

```java
//包名
package com.xxx.xxx;

import android.annotation.SuppressLint;
import android.content.Context;
import android.view.View;
import android.webkit.WebView;

import ohos.ace.adapter.capability.platformview.IPlatformView;

/**
 * MyWebView.
 * 自定义平台视图类
 * 该类继承 IPlatformView 接口并遵循 IPlatformView 协议，提供网页视图的封装
 */
public class MyWebView implements IPlatformView {
    private String id = "WebView";
    private WebView mWebView;
	//构造Android原生WebView组件
    @SuppressLint("SetJavaScriptEnabled")
    MyWebView(Context context) {
        mWebView = new WebView(context);
        String url = "https://gitcode.com/arkui-x";
        mWebView.getSettings().setJavaScriptEnabled(true);
        mWebView.getSettings().setDomStorageEnabled(true);
        mWebView.loadUrl(url);
    }
	// 返回关联的 View 对象（网页视图）
    @Override
    public View getView() {
        return mWebView;
    }
	// 清理资源的方法（当前为空实现，可根据需要扩展）
    @Override
    public void onDispose() {
        mWebView.destroy();
    }
	// 返回平台视图的唯一标识符
    @Override
    public String getPlatformViewID() {
        return id;
    }
}

```

自定义IPlatformView接口的实现类, 封装TextureMapView

```java
package com.xxx.xxx;

import android.content.Context;
import android.os.Bundle;
import android.view.View;

import com.amap.api.maps.TextureMapView;

import ohos.ace.adapter.capability.platformview.IPlatformView;

/**
 * MyMapView.
 * 自定义平台视图类
 * 该类继承 IPlatformView 接口并遵循 IPlatformView 协议，提供地图视图的封装
 */
public class MyMapView implements IPlatformView {
    TextureMapView mMapView;
    private String id = "MapView";
	//构造Android原生TextureMapView组件
    MyMapView(Context context, Bundle savedInstanceState) {
        mMapView = new TextureMapView(context);
        mMapView.onCreate(savedInstanceState);
    }
	// 返回关联的 View 对象（网页视图）
    @Override
    public View getView() {
        return mMapView;
    }
	// 清理资源的方法（当前为空实现，可根据需要扩展）
    @Override
    public void onDispose() {
        mMapView.onDestroy();
    }
	// 返回平台视图的唯一标识符
    @Override
    public String getPlatformViewID() {
        return id;
    }
}

```

自定义IPlatformView接口的实现类, 封装MediaPlayer

```java
package com.xxx.xxx;

import android.content.Context;
import android.content.res.AssetFileDescriptor;
import android.content.res.AssetManager;
import android.graphics.SurfaceTexture;
import android.media.MediaPlayer;
import android.view.Surface;
import android.view.TextureView;
import android.view.View;

import java.io.IOException;

import ohos.ace.adapter.ALog;
import ohos.ace.adapter.capability.platformview.IPlatformView;

/**
 * MyVideoView.
 * 自定义平台视图类
 * 该类继承 IPlatformView 接口并遵循 IPlatformView 协议，提供视频组件的封装
 */
public class MyVideoView implements IPlatformView, TextureView.SurfaceTextureListener {
    private String id = "VideoView";
    private TextureView textureView;
    private MediaPlayer mediaPlayer;
    private Context context;
	//构造Android原生MediaPlayer组件并关联TextureView进行渲染
    MyVideoView(Context context) {
        this.context = context;
        textureView = new TextureView(context);
        textureView.setSurfaceTextureListener(this);
        initMediaPlayer();
    }
	//视频组件相关初始化
    private void initMediaPlayer() {
        mediaPlayer = new MediaPlayer();
        AssetManager am = context.getAssets();
        try {
            AssetFileDescriptor afd = am.openFd("arkui-x/entry/resources/rawfile/video.mp4");
            mediaPlayer.setDataSource(afd.getFileDescriptor(), afd.getStartOffset(), afd.getLength());
        } catch (IOException e) {
            e.printStackTrace();
        }
        mediaPlayer.setOnPreparedListener(mp -> {
            mediaPlayer.start();
        });
        mediaPlayer.setLooping(true);
        mediaPlayer.prepareAsync();
    }
	// 返回关联的 View 对象（视频渲染组件）
    @Override
    public View getView() {
        return textureView;
    }
	// 清理资源的方法（当前为空实现，可根据需要扩展）
    @Override
    public void onDispose() {
        if (mediaPlayer != null) {
            mediaPlayer.release();
            mediaPlayer = null;
        }
    }
	// 返回平台视图的唯一标识符
    @Override
    public String getPlatformViewID() {
        return id;
    }
	
    @Override
    public void onSurfaceTextureAvailable(SurfaceTexture surface, int width, int height) {
        mediaPlayer.setSurface(new Surface(surface));
    }

    @Override
    public void onSurfaceTextureSizeChanged(SurfaceTexture surface, int width, int height) {

    }

    @Override
    public boolean onSurfaceTextureDestroyed(SurfaceTexture surface) {
        return false;
    }

    @Override
    public void onSurfaceTextureUpdated(SurfaceTexture surface) {

    }
}
```



### 自定义PlatformViewFactory接口的实现类

```java
package com.xxx.xxx;

import android.content.Context;
import android.os.Bundle;

import ohos.ace.adapter.capability.platformview.IPlatformView;
import ohos.ace.adapter.capability.platformview.PlatformViewFactory;

/**
 * MyPlatformViewFactory.
 * 继承 PlatformViewFactory 接口并遵循 PlatformViewFactory 协议，负责创建和管理平台视图实例
 */
public class MyPlatformViewFactory extends PlatformViewFactory {
    private Context context;
    private Bundle savedInstanceState;

    @Override
    public IPlatformView getPlatformView(String Id) {
        //通过ID标识符名称，创建对应组件
        if ("MapView".equals(Id)) {
            //地图组件
            return new MyMapView(context, savedInstanceState);
        } else if ("WebView".equals(Id)) {
            //网页组件
            return new MyWebView(context);
        } else if ("VideoView".equals(Id)) {
            //视频组件
            return new MyVideoView(context);
        }
        return null;
    }

    public void setContext(Context context) {
        this.context = context;
    }

    public void setSavedInstanceState(Bundle savedInstanceState) {
        this.savedInstanceState = savedInstanceState;
    }
}
```

### 自定义EntryEntryAbilityActivity 继承StageActivity，把PlatformViewFactory实现类注册到StageActivity中

```java
package com.xxx.xxx;

import android.os.Bundle;
import android.util.Log;
import ohos.stage.ability.adapter.StageActivity;

public class EntryEntryAbilityActivity extends StageActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        setInstanceName("com.xxx.xxx:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
        ...//其它功能代码
        //PlatformViewFactory实例对象创建并注册
        MyPlatformViewFactory platformViewFactory = new MyPlatformViewFactory();
        platformViewFactory.setContext(this);
        platformViewFactory.setSavedInstanceState(savedInstanceState);
        registerPlatformViewFactory(platformViewFactory);
    }
    @Override
    public void onConfigurationChanged(android.content.res.Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
    }
}

```

