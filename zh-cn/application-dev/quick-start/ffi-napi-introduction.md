# FFI能力(N-API)
## ArkUI-X中支持的N-API接口情况
Node-API是封装底层JavaScript运行时能力的一套Native接口。OpenHarmony的N-API组件对Node-API的接口进行了重新实现，ArkUI-X同样拥有这部分能力，目前支持部分接口，支持列表见[链接](../reference/native-lib/third_party_napi/napi.md)。

## ArkUI-X中N-API的使用场景
在OpenHarmony中，N-API接口可以实现ArkTS/TS/JS与C/C++(Native)之间的交互。ArkUI-X在此基础上进行了跨平台的拓展，开发者可在OpenHarmony/HarmonyOS/Android/iOS多个平台上使用N-API能力，完成跨语言工程开发。ArkUI-X中的N-API典型使用场景与OpenHarmony相同，即：
1. 通过N-API封装Native能力，暴露相应接口到ArkTS/TS/JS层，即ArkTS/TS/JS call Native。
2. Native代码中调用ArkTS/TS/JS提供的方法，即Native call ArkTS/TS/JS。

可参考[N-API在Android平台应用的使用指导](../tutorial/how-to-use-napi-on-android.md)与[N-API在iOS平台应用的使用指导](../tutorial/how-to-use-napi-on-ios.md)进行ArkUI-X Native工程开发。
