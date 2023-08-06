# 2023 Roadmap

2023年，ArkUI-X持续聚焦在iOS和Android平台上的能力构建和完善，关键特性集包括以下几个方面 (具体的特性范围可能会根据实际的演进情况有所调整)：

- 开发者工具

除了命令行工具（ACE Tools）支持之外，增加了DevEco Studio的集成开发环境支持，实现了应用代码开发、编译，调试，生成指定的OS平台包等相关能力。

- 应用开发模型

应用开发基于ArkTS的声明式开发范式，以及OpenHarmony的Stage开发模型。另外，支持模块化编译能力，降低增量编译时间、降低编译后的包体积，方便应用内代码共享。

- 跨语言调用

提供基于Node-API 的FFI（Foreign Function Interface）机制和平台桥接两种机制，用于API扩展和平台插件开发。

- 测试/调试能力

支持单元/UI/XTS集成测试。调试方面，目前支持OpenHarmony平台调试，Android和iOS平台暂不支持ArkTS调试。

- 混合开发

支持ArkUI-X集成到现有Android和iOS应用，以及Android的AAR和iOS的Framework平台库格式。

- 接口能力

ArkUI跨平台所支持的API Level从OpenHarmony API 10开始。相应的UI组件以及非UI相关的API接口能力，请参考具体的版本发布文档。

- 能力/架构演进

能力维度的演进，主要包括进一步增强DevEco Studio的能力（包括设备管理，Module级跨平台配置，ArkTS调试等）； 资源管理增强（包括在Android/iOS平台上可访问ArkUI的资源等），UI组件以及相关API能力的完善等。

架构维度的演进，主要包括架构解耦-组件API的按需加载，方舟编译器的AOT(Ahead Of Time)+PGO(Profile-Guided Optimization)能力在跨平台上的使能，以及在性能、内存、包体积的持续优化。

另外，我们也在探索和试验桌面平台（Windows/macOS/Linux）以及Web平台（浏览器）的支持，动态化内容加载机制， 2D/3D融合渲染等相关能力。

我们会持续结合典型的应用逐步推进，并通过实际应用的商用落地，进一步完善相应的功能，以及运行体验。也欢迎更多感兴趣的开发者加入进来，一起推进，一起创新，共建丰富的应用生态！