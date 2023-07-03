# N-API
## ArkUI-X中支持的N-API接口情况
Node-API是用于封装JavaScript能力为Native插件的API，独立于底层JavaScript，并作为Node.js的一部分。OpenHarmony的N-API组件对Node-API的接口进行了重新实现，ArkUI-X同样拥有这部分能力，目前支持部分接口，支持列表见[链接](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/native-lib/third_party_napi/napi.md)。

## ArkUI-X中N-API的使用场景
在OpenHarmony中，N-API接口可以实现ArkTS/TS/JS与C/C++之间的交互。ArkUI-X在此基础上进行了跨平台的拓展，开发者可在OpenHarmony/HarmonyOS/Android/iOS多个平台上使用N-API能力，完成跨语言工程开发。ArkUI-X中的N-API典型使用场景与OpenHarmony相同，可参考[OpenHarmony Native API在应用工程中的使用指导](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/napi/napi-guidelines.md)进行ArkUI-X Native工程开发。
