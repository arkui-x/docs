# ArkUI-X Roadmap in 2022

The singular focus of the ArkUI-X project in 2022 is on building the core facilities for the ArkUI cross-platform framework. It has now managed to extend ArkUI capabilities from OpenHarmony to iOS and Android platforms. By drawing from the provided toolchain, you can use one set of code across projects for different platforms: OpenHarmony, iOS, and Android. You can then build these projects into applications that run smoothly on target platforms. The key feature set includes the following aspects (the specific feature availability is subject to changes):

- Compiler toolchain

  The compiler toolchain comes with two types of command line interface (CLI) tools: one for ArkUI framework developers and one for application developers. The former can build ArkUI SDKs for different platforms (such as iOS and Android) for application packaging and distribution. (The OpenHarmony platform has ArkUI built in and therefore does not need a standalone ArkUI SDK.) The latter allows you to create platform-specific application projects, build applications for the target platform based on related SDK build projects, and start applications. The CLI tools work on computers running Windows [through Windows Subsystem for Linux (WSL)], macOS, or Linux.

- Platform bridging

  Platform bridging encompass the following platform-specific capabilities, including the application loading entry, lifecycle, event processing, window system, resource management, pasteboard, input method, camera, and various other advanced component access capabilities.

- UI component adaptation

  UI component adaptation allows for consistent drawing effects of UI components across platforms. In effect, this capability is about verifying the cross-platform rendering provided by ArkUI.

- Basic API capability adaptation

  Basic API capability adaptation involves the API extension mechanism and API implementation. The API extension mechanism is implemented based on the N-API and must be adapted to different platforms. API implementation includes the implementation of OpenHarmony JS APIs and Huawei Mobile Service (HMS) JS APIs on different platforms. It is carried out step by step in accordance with application requirements.

- Application embedding and integration

  Application embedding integration provides the capability of integrating target platform applications with ArkUI. Some target platform applications can be implemented based on the existing platform code, and the others can be implemented by introducing ArkUI. In this way, applications can be smoothly migrated to ArkUI.

- Debugging

  The debugging capability is mainly applicable to non-OpenHarmony platforms. It needs to be adapted and enabled accordingly on target platforms. The IDE plug-in construction will also be included in the future.

- XTS infrastructure

  The XTS infrastructure is used to verify the compatibility of ArkUI framework capabilities on different platforms. The same XTS test set (including UIs and APIs) is enabled on different OS platforms to help ensure the consistency of ArkUI framework capabilities across these platforms.

- Architecture evolution

  Architecture evolution mainly includes maintainability enhancement (architecture decoupling – on-demand loading of component APIs, hierarchical abstraction – abstraction of the rendering backend for smooth transition, and more), and improvement in performance, memory, and package volume. The evolution is subject to the evolution of ArkUI on OpenHarmony (minimum update, parallel reconstruction, rendering data reconstruction, and 2D-3D fusion) and the cross-platform service requirements.

## Current Progress

By November 2022, the ArkUI-X project has released a Beta version, a milestone in its journey. This version mainly packs the following capabilities:

- Basic compiler toolchain, which can generate ArkUI SDKs for iOS/Android to create applications that run on the target platform.
- Basic platform bridging capabilities, including the iOS/Android application loading entry points, lifecycle, event processing, and window system.
- Basic UI component adaptation capabilities, covering common UI components (for details, see [ArkUI-X App Samples](https://gitcode.com/arkui-x/samples)).
- XTS infrastructure, including common UI tests.

This milestone would not have been possible without our amazing partners: **Midea IoT mobile technical team** and **Alibaba Xianyu (Idle Fish) technical team**. Although it is not yet ready for prime time, it already has the infrastructure of the cross-platform framework. In the coming future, we will launch a version every three months revolving around the preceding key characteristics to better accommodate service needs and improve the user experience. We would like to invite developers enthusiastic about application development to join us in building a better application ecosystem.
