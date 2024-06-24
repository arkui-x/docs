# Platform Differentiation

## Introduction

In cross-platform development, you write a set of application code in ArkTS and run it on devices of different platforms, such as Android, iOS, and OpenHarmony (including commercial releases based on OpenHarmony, such as HarmonyOS Next). The need for platform differentiation arises when platform-specific service logic or APIs that do not support cross-platform calls are used. Read on to discover when and how you can perform platform differentiation, which is only available in the code running stage currently.

## Use Cases and Capabilities

### When to Use

Platform differentiation applies to the following two typical scenarios:

1. The service logic varies according to the platform.
2. One or more APIs that do not support cross-platform calls are used. In this case, while the APIs can be called on OpenHarmony, you need to perform differentiated processing for them on other platforms through the bridge mechanism.

### Identifying the Platform Type

Call **let osName: string = deviceInfo.osFullName;** to obtain the OS name. This API returns the following value of **osName**, depending on the platform it is called:

+ OpenHarmony: **OpenHarmony *XXX***
+ Android: **Android *XXX***
+ iOS: **iOS *XXX***

The following is an example:

```ts
test() {
  let osName: string = deviceInfo.osFullName;
  console.log('osName = ' + osName);
  if (osName.startsWith('OpenHarmony')) {
    // Service logic on OpenHarmony
  } else if (osName.startsWith('Android')) {
    // Service logic on Android
  } else if (osName.startsWith('iOS')) {
    // Service logic on iOS
  }
}
```

### Processing Non-cross-platform APIs

Non-cross-platform APIs do not support cross-platform calls. If you use them in your cross-platform project, the IDE will report an error during compilation. In the following example, the non-cross-platform **wifiManager.isWifiActive()** is called to check whether Wi-Fi is enabled.

The sample code is as follows:

```ts
  test2(){
   let isActive = wifiManager.isWifiActive();
  }
```

The IDE reports the following error:

```shell
> hvigor ERROR: Failed :feature:default@CompileArkTS... 
> hvigor ERROR: ArkTS Compiler Error
ERROR: ArkTS:ERROR File: D:/work/git/play-arkuix/Test_ACE/feature/src/main/ets/pages/Index.ets:64:31
 'isWifiActive' can't support crossplatform application.

COMPILE RESULT:FAIL {ERROR:2}
> hvigor ERROR: BUILD FAILED in 10 s 753 ms 
```

In this case, you can write the involved API to a .ts file and add **// @ts-ignore** or **// @ts-nocheck** above the API to exclude it from linting. Run this logic on the OpenHarmony platform only; use the bridge mechanism for processing on Android and iOS platforms. The sample code is as follows:

1. Create a **WiFiUtil.ts** file and exclude the concerned API from linting.

```ts
import wifiManager from '@ohos.wifiManager'

export class WiFiUtil {
  static isActive(): boolean {
    //@ts-ignore
    return wifiManager.isWifiActive();
  }
}
```

2. Based on the differentiated logic of platforms, use the [bridge mechanism](platform-bridge-introduction.md) to connect to the service logic of the Android or iOS platform.

```ts
checkTestWiFi(): void {
  let osName: string = deviceInfo.osFullName;
  console.log('osName = ' + osName);
  if (osName.startsWith('OpenHarmony')) {
    // OpenHarmony platform
    let isActive = WiFiUtil.isActive();
    this.message = isActive ? 'Connected': 'Not connected';
  } else {
    // On the Android and iOS platforms, transfer data to the native platform.
    let bridge = Bridge.createBridge('Bridge');
    bridge.callMethod('isWiFiActive').then((res) => {
      // Service logic processing
    }).catch(() => {

    })
  }
}
```
