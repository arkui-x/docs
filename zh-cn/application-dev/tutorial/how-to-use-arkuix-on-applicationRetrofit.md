# 跨平台应用改造指南



## 一、现状与诉求

&emsp;&emsp;随着 HarmonyOS Next 5.0 版本正式发布，众多开发者基于 ArkTS 语言为 HarmonyOS Next 系统开发了大量应用，这极大地丰富了 HarmonyOS 的生态。越来越多的应用上线，也给开发者带来了挑战，开发者需要同时开发和维护适用于 HarmonyOS Next、Android、iOS 三个平台的应用程序。这不仅意味着开发工作量大幅增加，开发成本也随之上升，而且很难保持一致的交互体验。<br>
&emsp;&emsp;ArkUI-X 跨平台框架是基于 HarmonyOS 打造的跨端跨平台框架，能实现 “一次开发、三平台部署”。 基于ArkTS开发的HarmonyOS Next应用，配套ArkUI-X跨平台框架，可以快速改造为跨平台应用，缩短开发周期，同时还能确保应用在 HarmonyOS Next、Android、iOS 多个平台上，为用户提供一致的交互体验。
<br>&emsp;&emsp;本文重点介绍如何将HarmonyNext应用工程转换为跨平台工程，实现一码多平台。<br>



## 二、改造目标

- 从零开始或基于现有HarmonyOS Next App进行改造，使其可快速部署于Android、IOS平台。<br>
- 通过ArkUI-X框架Bridge能力实现ArkTS与原生平台交互。<br>



## 三、方案说明

​		依据[分层架构设计](https://developer.huawei.com/consumer/cn/doc/best-practices-V5/bpta-layered-architecture-design-V5)理念，分三部分阐述。

<img src=".\figures\ApplicationRetrofit_image_001.png" />



### 第一部分：**产品定制层**（products）

&emsp;&emsp;由于不同操作系统之间的**数据平台差异**等客观原因，部分功能基于各平台的独特能力来实现不可避免。基于此，在products层，建议开发者建立两个module，分别命名为harmonyos和arkuix。在不同的hap包中集成对应的独特功能模块，最终编译成独立的hap包，以此实现功能隔离的效果。<br>

&emsp;&emsp;当然，倘若在app的实际开发进程中确定所有功能在三端应用时均可实现，也就是不需要考虑平台差异性问题，可以直接采用单hap设计。开发者需要根据实际开发状况进行全面综合的考量。<br>

| module    | **编译产物：hap包** | **运行设备平台/系统能力支持** |
| --------- | ------------------- | ----------------------------- |
| harmonyos | harmonyos.hap       | HarmonyOS Next                |
| arkuix    | arkuix.hap          | Android                       |
| arkuix    | arkuix.hap          | iOS                           |

&emsp;&emsp;products层为App主module，其编译产物为[HAP](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/hap-package-V5)。作为应用的入口。一般保留有应用启动页和应用首页。由于采用了分包编译，需要开发者于harmonyos.hap（下面简称A包）和 arkuix.hap （下面简称B包）中各自独立开发应用启动页和应用首页（下面简称Pages），带来额外的开发量。基于此，建议开发者进行 products层 设计时考虑以下几点：<br>

- 共性考虑：对于App来说，A包和B包都存在”设置页面、我的页面、登录页面“（见上图），这些共性的代码和文件如果分别存放于A包和B包会导致大量的冗余代码，也不利于后期维护，因此建议对其进行抽象，形成一个独立的模块存放（features层模块main），通过module依赖的方式使用。可参考案例：[案例解析](#四、案例解析)。<br>
- 差异性考虑：对于App来说，仅A包存在”发现页面“（见上图），如果B包运行至”发现页面“时会产生不可预知的后果。可参考案例：[案例解析](#四、案例解析)。<br>
  - B包的Pages设计时删除相关的页面元素，用户使用基于B包的app时不感知”发现页面“。<br>
  - 相关的数据结构、方法函数等应重新设计，可以抽象的部分进行抽象，存于features层模块main；无法抽象的部分各自实现。<br>



### 第二部分：**基础特性层**（features）

&emsp;&emsp;Features层属于App的特性界面层，其编译产物为[HAR](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/har-package-V5)/[HSP](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/in-app-hsp-V5)包。在该层级中，包含了应用的所有UI界面、弹窗、媒体图片等元素，这些都是能够被用户直接感知并进行操作的。此层是借助HarmonyOS的ArkUI组件以及相关能力来进行设计与开发的，并且ArkUI-X框架确保了在Android/iOS与HarmonyOS Next上能够拥有相同的展示效果和交互体验。<br>

​		（1） 开发者进行设计时需首先考虑ArkUI-X框架的实际适配状况，使用支持跨平台的UI控件、属性、方法进行跨平台开发，因为在UI组件方面存在的差异，是无法借助Bridge能力来弥补的。 关于ArkUI-X框架组件适配情况可参考：[ArkTS声明式开发范式跨平台支持列表](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/reference/arkui-ts/README.md)。可参考案例：[案例解析](#四、案例解析)。<br>

​		（2） API接口的使用：在使用API接口时可能会遇到以下两种情况：1、API不支持跨平台。2、API在Android、IOS平台支持能力有差异（具体信息可参考相应的API文档）。因此不建议开发者在features层直接调用API接口实现功能。应尽可能对其抽象，于commons层创建功能模块实现，使用时调用commons层相应功能模块接口。具体实现详见“第三部分：公共能力层（commons）”。<br>

​		（3）共通组件：在实际开发过程中，可以抽象出部分满足多种场景的共通UI。可以在commons层创建模块“**uicomponents**”，将共通UI抽象并实现，实现代码复用的效果。详见[架构图commons层 uicomponents](#三、方案说明)。<br>

​		（4）应注意，features层应合理设计模块，谨慎处理模块间依赖关系，避免循环依赖等问题。<br>

#### 模块main		

&emsp;&emsp;Products层harmonyos.hap（下面简称A包）和 arkuix.hap （下面简称B包）这样的双hap设计解决了因设备平台差异导致的应用功能差异的问题。但是由此引入了新问题：如果对Products层代码进行量化，那么**共性代码**（A包和B包可复用的一套代码）是要远远多于**差异性代码**（A包和B包需各自进行设计，无法复用）的。开发者在后续的开发和维护过程中，每次都需要同时修改分别部署于A包和B包的共性代码。由此产生大量冗余代码和不必要的工作量。<br>

&emsp;&emsp;为解决该问题，我们设计了模块main。模块main使用[HAR](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/har-package-V5)。通过将共性的部分抽取、实现。后续的开发和维护过程中，仅修改main模块即可，从而提升开发效率，减少冗余代码，优化代码结构。<br>

&emsp;&emsp;然而，需要特别注意的是，当products层采用单hap设计时，由于所有功能都集成在一个hap包中，因此无需额外创建模块main，以避免不必要的复杂性。

<img src=".\figures\ApplicationRetrofit_image_013.png" />

图片左侧为单hap设计，因为不存在差异性代码，所有代码集成于Products层phone.hap中，向下依赖features层。<br>

&emsp;&emsp;图示说明：

​			（1）phone.hap 同时依赖features1、features2、features3。

​			（2）features1、features2、features3同时依赖commons层。

图片右侧为双hap设计，其中差异性代码各自在harmonyos.hap和arkuix.hap中分别实现，实现功能隔离。共性代码抽象集成于features层模块main中，A包和B包共同依赖模块main，实现代码复用。<br>

&emsp;&emsp;图示说明：

​			（1）harmonyos.hap 依赖features1、features3。

​			（2）arkuix.hap 依赖features1、features3。

​			（3）harmonyos.hap和arkuix.hap同时依赖main。

​			（4）main依赖features2。

​			（5）features1、features2、features3同时依赖commons层。



### 第三部分：公共能力层（commons）

&emsp;&emsp;commons层是App的能力集合体，其编译产物为[HAR](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/har-package-V5)/[HSP](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V5/in-app-hsp-V5)包，主要用于阐述App的核心功能与附加服务。它向上能够为features层和products层直接给予能力方面的支持，向下则依靠运行设备平台的系统能力。ArkUI - X框架的Bridge能力，为功能模块直接调用Android/iOS平台的能力提供了有力的支撑。<br>

&emsp;&emsp;需要注意的是，commons层应当合理规划模块设置，谨慎对待模块间的依赖关系，避免出现循环依赖等问题。<br>

&emsp;&emsp;建议开发者首先考虑使用ArkUI-X框架已有进行开发，可参考：[ArkUI-X框架 API参考](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/README.md#api参考)。<br>

&emsp;&emsp;根据当前ArkUI-X框架的适配现状，可分为三种改造方式，结合[架构图commons层 NetWork](#三、方案说明)进行说明。至于工程中具体的文件部署细节详见：[工程目录结构设计](#第四部分工程目录结构设计)。<br>

&emsp;&emsp;**说明:** 示例代码主要展示整体流程、架构。相关代码细节如接口定义、函数功能实现等需开发者根据实际进行开发填充。<br>

#### 场景1：

**ArkUI-X框架当前已经支持跨平台的**

（1）创建实例类： "NetWork"，内部定义功能方法，提供给features层。方法直接使用HarmonyOS提供d.ts接口实现

```tsx
//NetWork.ets
import http from '@ohos.net.http';

/*
 * 定义class NetWork 声明方法request。提供给features和products层使用
 * */
export class NetWork {
  private httpRequest: http.HttpRequest;

  constructor() {
    this.httpRequest = http.createHttp();
  }

    /*
 	* 方法request，使用@ohos.net.http
 	* */
  public request(): void {
    this.httpRequest.request("EXAMPLE_URL", (err: Error, data: http.HttpResponse) => {
      if (!err) {
        console.info('Result:' + data.result);
      } else {
        console.info('error:' + JSON.stringify(err));
      }
    });
  }
}
```

（2）features层使用

```tsx
//index.ets
import {NetWork, NetWorkInterface} from '@NetWork';

@Entry
@Component
struct Index {
  @State message: string = '网络请求'

  build() {
    Row() {
      Column() {
        Button(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)
          .onClick(() => {
            let netWork = new NetWork();
            netWork.request();
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

#### 场景2：

**ArkUI-X框架当前不支持跨平台，但是可以通过Bridge平台桥接，三端效果一致的**

​		关于Bridge平台桥接的详细使用请参考资料：[Bridge平台桥接](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/quick-start/platform-bridge-introduction.md)。

（1）抽象出功能接口类：NetWorkInterface。

- **明确职责**：接口类定义了一组方法签名，明确了实现该接口的类必须提供的功能。
- **解耦**：通过接口，调用层和实现层之间解耦。

```tsx
//NetWorkInterface.ets
/*
 * 定义interface NetWorkInterface 声明方法request。
 * */
interface NetWorkInterface {
  request(): void;
}
```

（2）创建两个Class继承 NetWorkInterface，通过继承接口，实现接口中定义的所有方法，进而完成模块NetWork的功能实现。应注意，NetWorkLocal和 NetWorkArkUIX  **不应**暴露给上层。

| 实例Class名称 | 负责平台       | 功能实现方式                             |
| ------------- | -------------- | ---------------------------------------- |
| NetWorkLocal  | HarmonyOS Next | 使用HarmonyOS相关d.ts功能接口实现        |
| NetWorkArkUIX | Android、iOS   | 通过Bridge平台桥接调用平台侧原生方法实现 |

```tsx
//NetWorkLocal.ets
/*
 * 引用@ohos.net.http接口定义
 * */
import http from '@ohos.net.http';
import { NetWorkInterface } from 'NetWorkInterface.ets';

/*
 * 定义class NetWorkLocal 实现NetWorkInterface，
 * */
export class NetWorkLocal implements NetWorkInterface {
  private httpRequest: http.HttpRequest;

  constructor() {
    this.httpRequest = http.createHttp();
  }

  /*
   * 方法request，使用@ohos.net.http实现
   * */
  public request(): void {
    this.httpRequest.request("EXAMPLE_URL", (err: Error, data: http.HttpResponse) => {
      if (!err) {
        console.info('Result:' + data.result);
      } else {
        console.info('error:' + JSON.stringify(err));
      }
    });
  }
}
```

```tsx
//NetWorkArkUIX.ets
/*
 * 引用@arkui-x.bridge接口定义
 * */
import bridge from '@arkui-x.bridge';
import { NetWorkInterface } from 'NetWorkInterface.ets';

/*
 * 定义class NetWorkArkUIX 实现NetWorkInterface
 * */
export class NetWorkArkUIX implements NetWorkInterface {
  private bridgeImpl: bridge.BridgeObject;

  constructor() {
    this.bridgeImpl = bridge.createBridge('NetWorkUtil');
  }

  /*
   * 方法request，使用@arkui-x.bridge调用原生接口能力实现
   * */
  public request(): void {
    this.bridgeImpl?.callMethod("request", "EXAMPLE_URL").then((data: bridge.ResultValue) => {
      console.info('Result:' + data as string);
    }).catch((err: BusinessError) => {
      console.info('error:' + JSON.stringify(err));
    })
  }
}
```

（3）定义 实例类 PlatformInfo, 数据 PlatformTypeEnum：用于区分当前设备平台。**开发者可直接使用**。

```tsx
// PlatformInfo.ets
import deviceInfo from '@ohos.deviceInfo';

export enum PlatformTypeEnum {
  HARMONYOS = 'HarmonyOS Platform',
  ANDROID = 'Android Platform',
  IOS = 'iOS Platform',
  UNKNOWN = 'Unknown Platform',
}

export class PlatformInfo {
  static getPlatform(): PlatformTypeEnum {
    let osFullNameInfo: string = deviceInfo.osFullName;
    let platformName: string = osFullNameInfo.split(' ')[0];
    if (platformName.includes("OpenHarmony")) {
      return PlatformTypeEnum.HARMONYOS;
    } else if (platformName.includes("Android")) {
      return PlatformTypeEnum.ANDROID;
    } else if (platformName.includes('iOS')) {
      return PlatformTypeEnum.IOS;
    } else {
      return PlatformTypeEnum.UNKNOWN;
    }
  }
}
```

（4）创建实例类： NetWork。暴露给features层和products层，于运行时根据设备平台创建 NetWorkLoca l和 NetWorkArkUIX。

```tsx
//NetWork.ets
import { PlatformInfo, PlatformTypeEnum } from 'PlatformInfo.ets';
import { NetWorkInterface } from 'NetWorkInterface.ets';
import { NetWorkLocal } from 'NetWorkLocal.ets';
import { NetWorkArkUIX } from 'NetWorkArkUIX.ets';

export class NetWork {
  public static getNetWork(): NetWorkInterface {
    let platform: PlatformTypeEnum = PlatformInfo.getPlatform();
    if (platform == PlatformTypeEnum.ANDROID || platform == PlatformTypeEnum.IOS) {
      return new NetWorkArkUIX();
    } else {
      return new NetWorkLocal();
    }
  }
}
```

（5）平台侧处理：需要分别考虑Android侧和iOS侧的实现方式

Android侧：

 1. 新增JAVA类：BridgeNetWorkUtil

    ```java
    // .arkui-x\android\app\src\main\java\com\example\test\BridgeSrc\BridgeNetWorkUtil.java
    
    public class BridgeNetWorkUtil extends BridgePlugin {
        public String name;
        private Context context;
    
        public BridgeNetWorkUtil(Context context, String name, BridgeManager bridgeManager) {
            super(context, name, bridgeManager);
            this.name = name;
            this.context = context;
        }
    
        public void request(String url) {
            //使用Android 原生方法实现方法
        }
    }
    ```

2. 应用启动初始化 BridgeNetWorkUtil

   ```java
   // .arkui-x\android\app\src\main\java\com\example\test\ArkuixEntryAbilityActivity.java
   
   public class ArkuixEntryAbilityActivity extends StageActivity {
       static Context context;
       static BridgeManager bridgeManager;
   
       /*
        * 初始化平台侧Bridge对象
        */
       private void initBridgeObject() {
           context = this;
           bridgeManager = getBridgeManager();
           new BridgeNetWorkUtil(context, "NetWorkUtil", bridgeManager);
       }
   
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           Log.i(TAG, "onCreate called");
           initBridgeObject();
           setInstanceName("com.huawei.hmos.world:arkuix:EntryAbility:");
           super.onCreate(savedInstanceState);
       }
   }
   ```

iOS侧：

1. 新增OC类：BridgeNetWorkUtil

   ```c++
   // BridgeNetWorkUtil.h
   
   #import <UIKit/UIKit.h>
   #import <libarkui_ios/BridgePlugin.h>
   
   NS_ASSUME_NONNULL_BEGIN
   
   @interface BridgeNetWorkUtil : BridgePlugin<IMessageListener, IMethodResult>
   - (void)request:(NSString *)uri;
   @end
   
   NS_ASSUME_NONNULL_END
   ```

   ```cpp
   // BridgeNetWorkUtil.m
   #import "BridgeNetWorkUtil.h"
   
   @implementation BridgeNetWorkUtil
   - (void)request:(NSString *)uri{
   	//使用iOS 原生方法实现方法
   }
   @end
   ```

2. 应用启动初始化 BridgeNetWorkUtil

   ```cpp
   // .arkui-x\ios\app\ArkuixEntryAbilityViewController.m
   
   #import "ArkuixEntryAbilityViewController.h"
   #import <libarkui_ios/BridgePlugin.h>
   #import <libarkui_ios/BridgePluginManager.h>
   #import "BridgeSrc/include/BridgeNetWorkUtil.h"
   
   @interface ArkuixEntryAbilityViewController ()
   @property (nonatomic, strong) BridgeNetWorkUtil* bridgeNetWorkUtil;
   @end
   
   @implementation ArkuixEntryAbilityViewController
   - (instancetype)initWithInstanceName:(NSString *)instanceName {
       self = [super initWithInstanceName:instanceName];
       if (self) {
           [self initBridgePlugin];
       }
       return self;
   }
   
   - (void)initBridgePlugin {
       self.bridgeNetWorkUtil = [
           [BridgeNetWorkUtil alloc] initBridgePlugin:@"NetWorkUtil" bridgeManager:[selfgetBridgeManager]
       ];
       self.bridgeNetWorkUtil.messageListener = self.bridgeNetWorkUtil;
       self.bridgeNetWorkUtil.methodResult = self.bridgeNetWorkUtil;
   }
   
   - (void)viewDidLoad {
       [super viewDidLoad];
       self.edgesForExtendedLayout = UIRectEdgeNone;
       self.extendedLayoutIncludesOpaqueBars = YES;
   }
   
   @end
   ```


（6）features层使用：对于features来说，实际上仅可见 NetWork 和  NetWorkInterface；只需关注接口定义及功能，无需关注具体的实现和平台。

```tsx
//index.ets
import {NetWork, NetWorkInterface} from '@NetWork';

@Entry
@Component
struct Index {
  @State message: string = '网络请求'

  build() {
    Row() {
      Column() {
        Button(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)
          .onClick(() => {
            //仅知晓模块NetWork的方法声明，而不关注具体实现。
            NetWork.getNetWork().request();
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

#### 场景3：

**ArkUI-X框架当前不支持跨平台，且无法通过Bridge平台桥接实现的**

&emsp;&emsp;整体代码结构与设计流程与上述第二点一致，区别在于NetWorkArkUIX中的实现逻辑：为保证App正常运行，使用空实现，同时使用如输出错误日志信息、返回错误码、警告弹窗等方式提示调用者或使用者。

```tsx
//NetWorkArkUIX.ets
/*
 * 引用@arkui-x.bridge接口定义
 * */
import bridge from '@arkui-x.bridge';
import { NetWorkInterface } from 'NetWorkInterface.ets';

/*
 * 定义class NetWorkArkUIX 实现NetWorkInterface
 * */
export class NetWorkArkUIX implements NetWorkInterface {
  private bridgeImpl: bridge.BridgeObject;

  constructor() {
    this.bridgeImpl = bridge.createBridge('NetWorkUtil');
  }

  /*
   * 方法request，使用空实现，输出错误日志
   * */
  public request(): void {
    console.error('This feature is not supported by the current device platform')
    return;
  }
}
```

### 第四部分工程目录结构设计

&emsp;&emsp;阐述说明相关文件的目录结构设计。跨平台整体工程目录详细资料请参考[ArkUI-X应用工程结构说明](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/quick-start/package-structure-guide.md)。

```tsx
工程
├── .arkui-x
│   ├── android
│   │   └── app\src\main
│   │       ├── java\com\example\test
│   │       |	├── BridgeSrc			 			 // Android平台Bridge类目录
|   |       |   |	 └── BridgeNetWorkUtil.java		 // NetWork Android 原生实现类
│   │       |	├── ArkuixEntryAbilityActivity.java
│   │       |	└── MyApplication.java
│   │       └── AndroidManifest.xml
|   | 
│   ├── ios
│   │   ├── app
│   │   |	├── BridgeSrc				  			 // iOS平台Bridge类目录
|   |   |   |   ├──	include							 // iOS平台Bridge 原生实现类 .h 文件目录
|   |   |   |   |	 └── BridgeNetWorkUtil.h		 // NetWork iOS 原生实现类
|   |   |   |   └──	src								 // iOS平台Bridge 原生实现类 .m 文件目录
|   |   |   |   	 └── BridgeNetWorkUtil.m		 // NetWork iOS 原生实现类
│   │   |   ├── AppDelegate.m
│   │   |   ├── AppDelegate.h
│   │   |   ├── ArkuixEntryAbilityViewController.h
│   │   |   └── ArkuixEntryAbilityViewController.m
│   │   └── app.xcodeproj
│   └── arkui-x-config.json5
|    
├── AppScope
│   ├── resourcesA
│   └── App.json5
├── commons	
│   ├── network										// commons层级功能模块 network
│   │   ├── src\main\ets
│   │   │   ├── interface							// interface 文件目录						
|   |   |   |    └── NetWorkInterface.ets			  // network 模块功能接口定义
|   |   |   └── utils								// class 文件目录
|   |   |        ├── NetWork.ets					  // NetWork
|   |   |        ├── NetWorkArkUIX.ets				  // ArkUIX实现
|   |   |        └── NetWorkLocal.ets				  // HarmonyOS实现
│   │   ├── Index.ets
│   │   └── oh-package.json5
|   |
│   ├── uicomponents								// commons层级功能模块 uicomponents 通用UI组件
│   │   ├── src\main\ets
│   │   │   ├── common								// 常量、数据结构等定义文件目录
|   |   |   └── components							// 通用UI组件 文件目录
│   │   ├── Index.ets								// uicomponents 对外导出组件实例文件
│   │   └── oh-package.json5						// uicomponents 依赖项配置文件
|   |
│   ├── utils										// commons层级功能模块 utils 通用方法
│   │   ├── src\main\ets
│   │   │   ├── common								// 常量、数据结构等定义文件目录
|   |   |   └── utils								// class 文件目录
|   |   |        └── PlatformInfo.ets				  // 区分当前设备平台
│   │   ├── Index.ets								// utils 对外暴露接口导出文件
│   │   └── oh-package.json5						// utils 依赖项配置文件
│   └── 功能...
├── features
│   ├── main 								// 模块 main
│   |    ├── src\main
│   |    |	  ├── ets						
│   |    |	  |	   └── model
│   |    |	  |	   		└── SplashSource.ets
│   |    |	  ├── resources
│   |    |	  └── module.json5
│   |    ├── build-profile.json5			
│   |    ├── Index.ets						// 模块对外暴露接口导出文件
│   |    └── oh-package.json5				// 模块依赖项配置文件
│   └── 特性...
├── products
│   └── phone
│       ├── arkuix							// 入口Module：Android、IOS平台
│       |	├── src\main
│       |	|	├── ets
│       |	|	|	├── entryability
│       |	|	|	|	└── EntryAbility.ets
│       |	|	|	└── pages
│       |	|	|		├── SplashPage.ets		// 应用启动页
│       |	|	|		└── MainPage.ets		// 应用首页
│       |	|	├── resources
│       |	|	└── module.json5
│       |	├── build-profile.json5
│       |	└── oh-package.json5				// 模块依赖项配置文件
│       └── harmonyos						// 入口Module：HarmonyOS Next平台 
│        	├── src\main
│       	|	├── ets
│       	|	|	└── pages
│       	|	|		├── SplashPage.ets		// 应用启动页
│       	|	|		└── MainPage.ets		// 应用首页
│       	|	├── resources
│       	|	└── module.json5
│       	├── build-profile.json5
│       	└── oh-package.json5				// 模块依赖项配置文件
├── build-profile.json5
└── oh-package.json5
```



## 四、案例解析

&emsp;&emsp;目前，已经有按照方案完成整体改造的4个Sample作为完整案例。

| **应用描述** | **链接**                                                     |
| ------------ | ------------------------------------------------------------ |
| 鸿蒙世界     | [HMOSWorld](https://gitcode.com/arkui-x/samples/tree/master/CodeLab/HMOSWorld) |
| 溪村小镇     | [OxHornCampus](https://gitcode.com/arkui-x/samples/tree/master/CodeLab/OxHornCampus) |
| 音乐专辑     | [MusicHome](https://gitcode.com/arkui-x/samples/tree/master/CodeLab/MusicHome) |
| 购物应用     | [MultiShopping](https://gitcode.com/arkui-x/samples/tree/master/CodeLab/MultiShopping) |

&emsp;&emsp;下面以实际改造过程中遇到的经典问题进行案例详解。完整代码可通过上述表格链接查找。

### Products共性拆分

&emsp;&emsp;在拆分原工程products模块为两个hap时，将可以复用的代码进行抽象，存于features层main，被hap依赖使用。

&emsp;&emsp;首先识别可以复用的代码逻辑部分，以[溪村小镇](https://gitcode.com/arkui-x/samples/tree/master/CodeLab/OxHornCampus)为例，应用启动页会轮播三张图片，而图片源的数据结构作为可复用部分，将其存放于features层main中。

<img src=".\figures\ApplicationRetrofit_image_002.png" />



模块main 对外暴露 数据结构

```tsx
// OxHornCampus\features\main\Index.ets

// 对外暴露数据源
export { splashImages } from './src/main/ets/viewmodel/SplashModel';
```



arkuix和harmonyos使用时添加对模块main的依赖，即可访问数据。

```json
// OxHornCampus\products\phone\arkuix\oh-package.json5
// harmonyos同理

{
  "name": "arkuix",
  "version": "1.0.0",
  "description": "Please describe the basic information.",
  "main": "",
  "author": "",
  "license": "",
  "dependencies": {
    "@ohos/utils": "file:../../../commons/utils",
    "@ohos/map": "file:../../../features/map",
    "@ohos/zones": "file:../../../features/zones",
    "@ohos/train": "file:../../../features/train",
    //添加模块依赖
    "@ohos/main": "file:../../../features/main", 
  }
}
```



###  Products差异性性拆分

&emsp;&emsp;以[鸿蒙世界](https://gitcode.com/arkui-x/samples/tree/master/CodeLab/HMOSWorld)为例，HarmonyOS Next设备上应用持有5个tab页，其中 tabs“溪村挑战赛” 使用了harmonyos的独有能力进行UI设计。由于无法通过Bridge实现跨平台改造，因此需要在Android/iOS平台部署时删除该tab页相关元素，同时相关数据结构等根据平台独立设计，分别存放于harmonyos.hap 和 arkuix.hap。

&emsp;&emsp;arkuix侧不存在“CHALLENGE”数据项。harmonyos侧存在“CHALLENGE”数据项。

<img src=".\figures\ApplicationRetrofit_image_003.png" />

最终实现效果

| harmonyos包展示效果，存在tab页“溪村挑战赛”                | arkuix包展示效果，没有tab页“溪村挑战赛”                   |
| --------------------------------------------------------- | --------------------------------------------------------- |
| <img src=".\figures\ApplicationRetrofit_image_004.jpg" /> | <img src=".\figures\ApplicationRetrofit_image_005.jpg" /> |
|                                                           |                                                           |

### 使用支持跨平台的UI控件、属性、方法进行跨平台开发

&emsp;&emsp;在[音乐专辑](https://gitcode.com/arkui-x/samples/tree/master/CodeLab/MusicHome)中，当音乐播放时，播放控制栏的音乐图标会执行旋转动画，实际上HarmonyOS Next与Android/iOS使用了两套逻辑实现。

在HarmonyOS Next上。使用[@ohos.graphics.displaySync (可变帧率)](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/js-apis-graphics-displaysync-V5)实现动画效果。

```tsx
// DisplaySyncLocal.ets

import { displaySync } from '@kit.ArkGraphics2D';
import { DisplaySyncInterface } from '../Interface/DisplaySyncInterface';

export class DisplaySyncLocal implements DisplaySyncInterface {
  private static instance: DisplaySyncLocal;
  private backDisplaySyncSlow: displaySync.DisplaySync | undefined = undefined;

  public static getInstance(): DisplaySyncInterface {
    if (!DisplaySyncLocal.instance) {
      DisplaySyncLocal.instance = new DisplaySyncLocal();
    }
    return DisplaySyncLocal.instance;
  }

  public createAnimate(range: ExpectedFrameRateRange, frame: () => void): void {
    this.backDisplaySyncSlow = undefined;
    this.backDisplaySyncSlow = displaySync.create();
    this.backDisplaySyncSlow.setExpectedFrameRateRange(range);
    this.backDisplaySyncSlow.on('frame', frame);
  }

  public deleteAnimate(frame: () => void): void {
    if (this.backDisplaySyncSlow != undefined) {
      this.backDisplaySyncSlow?.off('frame', frame);
      this.backDisplaySyncSlow = undefined;
    }
  }

  public startAnimate(): void {
    if (this.backDisplaySyncSlow != undefined) {
      this.backDisplaySyncSlow?.start();
    }
  }

  public stopAnimate(): void {
    if (this.backDisplaySyncSlow != undefined) {
      this.backDisplaySyncSlow?.stop();
    }
  }
}
```

由于当前ArkUI-X框架未适配这套方法，在arkui-x侧实际上使用了[@ohos.animator (动画)](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/js-apis-animator-V5)实现动画效果。

```tsx
// DisplaySyncArkUIX.ets

import { Animator, AnimatorResult } from '@kit.ArkUI';
import { DisplaySyncInterface } from '../Interface/DisplaySyncInterface';

export class DisplaySyncArkUIX implements DisplaySyncInterface {
  private static instance: DisplaySyncArkUIX;
  private backAnimator: AnimatorResult | undefined = undefined;

  public static getInstance(): DisplaySyncInterface {
    if (!DisplaySyncArkUIX.instance) {
      DisplaySyncArkUIX.instance = new DisplaySyncArkUIX();
    }
    return DisplaySyncArkUIX.instance;
  }

  public createAnimate(range: ExpectedFrameRateRange, frame: () => void): void {
    this.backAnimator = undefined;
    this.backAnimator = Animator.create({
      duration: 5000,
      easing: "linear",
      delay: 0,
      fill: "forwards",
      direction: "normal",
      iterations: -1,
      begin: 0,
      end: 1
    })
    this.backAnimator.setExpectedFrameRateRange(range);
    this.backAnimator.onFrame = frame;
  }

  public deleteAnimate(frame: () => void): void {
    if (this.backAnimator != undefined) {
      this.backAnimator.cancel();
      this.backAnimator = undefined;
    }
  }

  public startAnimate(): void {
    if (this.backAnimator != undefined) {
      this.backAnimator.play();
    }
  }

  public stopAnimate(): void {
    if (this.backAnimator != undefined) {
      this.backAnimator.pause();
    }
  }
}
```



## 五、关于DevEco Studio编译时报错问题解决

&emsp;&emsp;问题现象：DevEco Studio编译hap时报错：“ xxx can't support crossplatform application. ”

<img src=".\figures\ApplicationRetrofit_image_008.png" />

&emsp;&emsp;问题解析：由于使用了跨平台工程模版，DevEco Studio在进行静态编译检查时会检查跨平台标签“@crossplatform”。而在工程中会使用一些当前不支持跨平台的HarmonyOS 接口导致静态编译检查失败。

​		解决方法：

​		1、找到 IDE 里配套 OH-SDK；如果是HarmonyOS Next开发，则是HarmonyOS 里带的oh-sdk。

&emsp;&emsp;简便方法：前提需保证工程使用SDK为正确的。使用DevEco Studio打卡任一工程，在工程中打开并查看任一d.ts文件，于文件名右键点击-->选择 打开范围-->选择 Explorer 点击，打开的文件窗口即为当前工程所使用的SDK路径，于文件窗口回到SDK根目录执行第2步。

<img src=".\figures\ApplicationRetrofit_image_009.png" />

​		2、找到文件：" api_check_util.js "。文件在SDK中的相对路径为：

```
sdk\HarmonyOS-NEXT-DB1\openharmony\ets\build-tools\ets-loader\lib\fast_build\system_api\api_check_utils.js
```

​		3、在文件" api_check_util.js "中搜索关键字：**CROSSPLATFORM_TAG_CHECK_ERROER**，将其后边的 **DiagnosticCategory.Error** 修改为 **DiagnosticCategory.Warning**。

​		4、回到DevEco Studio 如果当前工程已编译过，先执行clean操作；之后执行操作： 点击文件-->选择 清理缓存 点击--> 勾选选项 -->  点击清除并重新启动。

<div style="text-align: center;">
  <img src=".\figures\ApplicationRetrofit_image_012.png" alt="" style="width: 30%; margin: 1%; display: inline-block;">
  <img src=".\figures\ApplicationRetrofit_image_010.png" alt="" style="width: 30%; margin: 1%; display: inline-block;">
  <img src=".\figures\ApplicationRetrofit_image_011.png" alt="" style="width: 30%; margin: 1%; display: inline-block;">
</div>



## 六、约束与建议

- 本方案是依据ArkUI-X框架来实现的，应首先符合ArkUI-X框架的规格要求，详细内容可查看[ArkUI-X框架规格](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/tutorial/specification/framework-specification.md)。<br>

- 在应用UI方面存在的差异，是无法借助Bridge能力来弥补的。在此建议使用ArkUI-X框架中已经适配完毕的组件，这些组件功能相对稳定且较为全面。

  详细内容可查看[ArkTS声明式开发范式跨平台支持列表](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/reference/arkui-ts/README.md)、[ArkUI-X框架 API参考](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/README.md#api参考)。<br>

- 应用改造过程中可能涉及通过Bridge框架使用平台原生接口方法，使用时需满足相应的原生系统版本要求。<br>

