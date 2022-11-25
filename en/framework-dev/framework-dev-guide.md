# Framework Development Overview

The framework development documents provide guidance for you to set up the environment for the ArkUI-X project, and download, compile, and contribute code using the framework code provided by the ArkUI-X project master repository. Currently, the basic capability adaptation of the ArkUI development framework on Android, iOS, and OpenHarmony has been completed for the ArkUI-X project.

The documents are organized as follows:

## Getting Started

[Before You Start](quick-start/start-overview.md) helps you learn how to quickly get started with cross-platform application development.

You can learn how to set up the environment for the ArkUI-X project, download ArkUI-X source code, build an ArkUI-X project, and contribute code. You can also learn fundamentals of the ArkUI framework.

The development fundamentals include the overall design, and code structure and building of the ArkUI-X project.

## Development

The ArkUI framework provides a unified API extension mechanism, which reuses the [NAPI mechanism](../framework-dev/napi/napi-guidelines.md) and JS interface definitions of OpenHarmony. You can use the JS->C++/Java/ObjectC mutual-invocation mechanism to extend JS APIs and implement OpenHarmony APIs on Android and iOS, and develop third-party plug-ins.

## Samples

To make you better understand how functions work together and jumpstart your application development projects, we provide stripped-down, real-world [samples](https://gitee.com/arkui-x/samples). These samples can be directly run through ArkUI-X CLI tools.

With the example tutorial, you can add and invoke your JS APIs to the sample project.

## API References

The API references encompass the components and APIs available in ArkUI-X, helping you locate and use APIs more effectively.

They are organized as follows:

- [ArkTS-based Declarative Development Paradigm](https://gitee.com/openharmony/docs/blob/master/en/application-dev/reference/arkui-ts/Readme-EN.md)
- [API Reference (ArkTS and JS APIs)](../application-dev/reference/apis/readme.md)
- Platform Integration
  - [Android](../application-dev/reference/arkui-for-android/readme.md)
  - [iOS](../application-dev/reference/arkui-for-ios/readme.md)
