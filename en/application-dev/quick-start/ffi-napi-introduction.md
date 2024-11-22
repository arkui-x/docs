# FFI (Node-API)
## Node-API Supported by ArkUI-X
Node-API provide a set of native interfaces that encapsulate the underlying JavaScript runtime capabilities. The OpenHarmony Node-API component re-implements the Node-API interfaces. ArkUI-X also has this capability and supports [some interfaces](../reference/native-lib/third_party_napi/napi.md).

## Using Node-API in ArkUI-X
In OpenHarmony, Node-API implements interaction between ArkTS/TS/JS and C/C++ (native). ArkUI-X extends Node-API across platforms. You can use Node-API capabilities on OpenHarmony, HarmonyOS, Android, and iOS platforms to complete cross-language project development. The typical application scenarios of Node-API in ArkUI-X are the same as those in OpenHarmony. That is, you can:
1. Use Node-API to encapsulate native capabilities and expose the interfaces to the ArkTS/TS/JS layer, to make ArkTS/TS/JS call native APIs.
2. Call ArkTS/TS/JS methods from native code, that is, make native APIs call ArkTS/TS/JS.

For details about how to develop a native project on ArkUI-X, see [How to Use Node-API on Android](../tutorial/how-to-use-napi-on-android.md) and [How to Use Node-API on iOS](../tutorial/how-to-use-napi-on-ios.md).
