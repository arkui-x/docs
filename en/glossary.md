# Glossary

## A

- ### ArkUI

  ArkUI is a UI development framework for building OpenHarmony applications. It provides simple and natural UI syntax and UI development infrastructure including UI components, animation mechanisms, and event interaction, to meet the visualized GUI development requirements of application developers. For more information, see [ArkUI](https://developer.harmonyos.com/en/develop/arkUI/).

- ### ArkUI-X

  ArkUI-X extends the ArkUI framework to multiple OS platforms. Currently, it supports OpenHarmony, HarmonyOS, Android, and iOS. More OS platforms will be supported in the future. With ArkUI-X, you can use a set of main code to develop high-performance applications supporting multiple platforms.

- ### ACE

  ArkUI Cross-Platform Environment (ACE) provides a set of command line tools (ACE Tools) for ArkUI-X application developers to check the development environment, create projects, and build, pack, install, and debug applications.

## N

- ### Node-API

  Node-API provides APIs to encapsulate JavaScript (JS) capabilities as a native plug-in. It is independent of the underlying JS and is maintained as part of Node.js. The N-API component of ArkUI-X re-implements Node-API interfaces. Currently, it supports [some interfaces](./application-dev/reference/native-lib/third_party_napi/napi.md) in the Node-API standard library.

## P

- ### Platform Bridge

  The platform bridge (@arkui-x.bridge) transmits messages between the client (ArkUI) and the platform (Android or iOS). It also enables ArkTS to call platform APIs and the platform to call ArkTS methods.
