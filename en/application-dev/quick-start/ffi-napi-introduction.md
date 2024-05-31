# FFI (N-API)
## N-APIs Supported by ArkUI-X
Node-APIs provide a set of Native interfaces that encapsulate the underlying JavaScript runtime capabilities. The OpenHarmony N-API component re-implements the Node-APIs. ArkUI-X also has this capability and supports [some interfaces](../reference/native-lib/third_party_napi/napi.md).

## Application Scenarios of N-APIs in ArkUI-X
In OpenHarmony, the N-APIs implement interaction between ArkTS/TS/JS and C/C++ (Native). ArkUI-X extends the N-APIs across platforms. You can use N-API capabilities on OpenHarmony, HarmonyOS, Android, and iOS platforms to complete cross-language project development. The typical application scenarios of N-APIs in ArkUI-X are the same as those in OpenHarmony. That is, you can:
1. Use N-APIs to encapsulate Native capabilities and expose the interfaces to the ArkTS/TS/JS layer, to make ArkTS/TS/JS call Native APIs.
2. Call the methods provided by ArkTS/TS/JS in Native code, that is, make Native APIs call ArkTS/TS/JS.

For details about how to develop a Native project on ArkUI-X, see [How to Use N-APIs on Android](../tutorial/how-to-use-napi-on-android.md) and [How to Use N-APIs on iOS](../tutorial/how-to-use-napi-on-ios.md).
