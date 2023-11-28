# 平台差异化

## 简介

由于跨平台使用场景是一套ArkTS代码运行在多个终端设备上，如Android、iOS、HarmonyOS Next。当不同平台业务逻辑不同，或使用了不支持跨平台的API，就需要根据平台不同进行一定代码差异化适配。当前仅支持在代码运行态进行差异化，接下来详细介绍场景及如何差异化适配。

## 使用场景及能力

### 使用场景

平台差异化适用于以下两种典型场景：

1. 自身业务逻辑不同平台本来就有差异；
2. 在HarmonyOS Next上调用了不支持跨平台的API，这就需要在HarmonyOS Next上仍然调用对应API，其他平台通过Bridge桥接机制进行差异化处理；

### 判断平台类型

可以通过`let osName: string = deviceInfo.osFullName;`获取对应OS名字，该接口已支持跨平台，不同平台上其返回值如下:

+ HarmonyOS Next上，osName等于`OpenHarmony XXX`
+ Android上，osName等于`Android XXX`
+ iOS上，osName等于`iOS XXX`

示例如下:

```
  test() {
    let osName: string = deviceInfo.osFullName;
    console.log('osName = ' + osName);
    if (osName.startsWith('OpenHarmony')) {
      // 鸿蒙设备上业务逻辑
    } else if (osName.startsWith('Android')) {
      // Android设备上业务逻辑
    } else if (osName.startsWith('iOS')) {
      // iOS设备上业务逻辑
    }
  }
```

### 非跨平台API处理

在跨平台工程中如果调用非跨平台API，编译时IDE会触发拦截并报错。接下来以调用`wifiManager.isWifiActive()`判断WiFi开关是否打开为例，这个API当前是不支持跨平台的。

示例代码：

```
  test2(){
   let isActive = wifiManager.isWifiActive();
  }
```

IDE报错：

```
> hvigor ERROR: Failed :feature:default@CompileArkTS... 
> hvigor ERROR: ArkTS Compiler Error
ERROR: ArkTS:ERROR File: D:/work/git/play-arkuix/Test_ACE/feature/src/main/ets/pages/Index.ets:64:31
 'isWifiActive' can't support crossplatform application.

COMPILE RESULT:FAIL {ERROR:2}
> hvigor ERROR: BUILD FAILED in 10 s 753 ms 
```

此时可以将涉及到的API写到一个后缀为**.ts**文件，然后在不支持的API上面增加`// @ts-ignore`或`// @ts-nocheck`屏蔽告警，开发者需要保证只在OpenHarmony设备上才运行这一段逻辑，Android和iOS可以借用Bridge桥接机制处理，示例代码如下：

1. 新建一个`WiFiUtil.ts`，并忽略告警：

```
import wifiManager from '@ohos.wifiManager'

export class WiFiUtil {
  static isActive(): boolean {
    //@ts-ignore
    return wifiManager.isWifiActive();
  }
}
```

2. 根据不同平台差异化逻辑，Android和iOS上通过Bridge桥接到原生实现：

   ```
     checkTestWiFi(): void {
       let osName: string = deviceInfo.osFullName;
       console.log('osName = ' + osName);
       if (osName.startsWith('OpenHarmony')) {
         // OpenHarmony手机
         let isActive = WiFiUtil.isActive();
         this.message = isActive ? '已连接' : '未连接';
       } else {
         // Android、iOS手机,中转到原生
         let bridge = Bridge.createBridge('Bridge');
         bridge.callMethod('isWiFiActive').then((res) => {
           // 业务逻辑处理...
         }).catch(() => {
   
         })
       }
     }
   ```