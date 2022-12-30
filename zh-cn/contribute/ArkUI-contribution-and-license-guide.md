# 开发者代码贡献和许可证合规指南

## 目的

本规范明确了ArkUI-X项目的代码贡献的贡献协议与许可证合规指南，包括如下几个部分：

**（1）开发者原创声明**

**（2）贡献代码的许可证**

**（3）贡献文档的许可证**

**（4）第三方开源软件许可证要求和黑白名单建议**

## 范围

本规范仅适用于ArkUI-X社区，不适用于将ArkUI-X项目应用于个人或企业以开发其它产品的场景，也不适用分发第三方开源软件的场景（该场景参见第三方开源软件引入指导）。

**（1）开发者原创声明**

贡献者参与贡献，需要签署开发者原创声明。贡献者必须签署“开发者原创声明”（Developer Certificate of Origin，DCO），然后才能参与社区贡献。

DCO是由贡献者签署的对其贡献的作品原创性的声明和贡献者表示同意贡献的声明，DCO文本将完整引用Linux基金会发布的标准版本，您可以[查看DCO全文](https://developercertificate.org/)。

贡献者在每次提交贡献时均需签署DCO，DCO的首次签署请在[docs/zh-cn/contribute](signing-dco.md)路径下获取[signing-dco（Developer Certificate of Origin）](signing-dco.md)Markdown文件，将签署后的DCO Markdown文件发送至[contact@mail.arkui-x.cn](mailto:contact@mail.arkui-x.cn) 。（线上签署待开放）

DCO的后续签署将附随贡献提交流程进行。

**（2）贡献代码的许可证**

原则上，项目贡献代码均应提供源代码，并采用Apache License 2.0 (简称Apache-2.0)开源许可证进行分发，您可以[了解Apache-2.0原文](https://www.apache.org/licenses/LICENSE-2.0)。

**（3）贡献文档的许可证**

原则上，项目贡献文档应根据知识共享署名4.0 (Creative Commons Attribution 4.0，简称CC BY 4.0)国际公共许可协议进行版权许可，您[了解CC BY 4.0的概要](https://creativecommons.org/licenses/by/4.0/) ，[了解CC BY 4.0的完整协议内容](https://creativecommons.org/licenses/by/4.0/legalcode)。

**（4）三方开源软件许可证要求和黑白名单建议**

为便于第三方开源软件的维护与演进，在引入时请参考如下第三方开源软件的许可证考虑因素：

**自由再分发原则**，许可证不支持再分发的第三方依赖不能被接纳。

**许可证兼容原则**，若第三方依赖可能与ArkUI-X项目其它代码产生代码合并或混同，但该第三方依赖的许可证与混同/合并的代码的原许可证不兼容，不能接纳该第三方依赖。

**许可证义务可履行原则**，许可证包含无法履行或不易履行的开源许可证义务的第三方依赖不能被接纳，例如广告条款，啤酒条款。

**许可证包含不合理的限制或约束条款的第三方依赖不能被接纳**，例如商用限制条款，使用领域限制条款、歧视特别技术域条款或针对特定产品的条款。

许可证明确排除专利授权的，在有其他优选项的情况下不建议接纳，例如采用CC0许可证的第三方依赖。

对于采用弱传染性许可证的第三方依赖，如能确保履行该弱传染性许可证的义务且引用方式采取一定的隔离措施，可接纳该采用弱传染性许可证的第三方依赖。

**可接纳的第三方依赖的许可证**

原则上，许可证条款等同于Apache-2.0且其开源许可证义务能够被无障碍履行的许可证可以被接纳，经项目PMC审批同意如下第三方依赖的开源许可证白名单：

*  Academic Free License 3.0 (AFL-3.0)
*  Apache License 2.0 (Apache-2.0)
*  Apache Software License 1.1. Including variants:
*   PHP License 3.01
*   MX4J License
*  Boost Software License (BSL-1.0)
*  BSD License
*  3-clause BSD License
*  2-clause BSD License
*  DOM4J License
*  PostgreSQL License
*  ISC
*  ICU
*  MIT License (MIT) / X11
*  MIT No Attribution License (MIT-0)
*  Mulan Permissive Software License v2 (MulanPSL - 2.0)
*  The Unlicense
*  W3C License (W3C)
*  University of Illinois/NCSA
*  X.Net
*  zlib/libpng
*  FSF autoconf license
*  DejaVu Fonts (Bitstream Vera/Arev licenses)
*  OOXML XSD ECMA License
*  Microsoft Public License (MsPL)
*  Python Software Foundation License
*  Python Imaging Library Software License
*  Adobe Postcript(R) AFM files
*  Boost Software License Version 1.0
*  WTF Public License
*  The Romantic WTF public license
*  UNICODE, INC. LICENSE AGREEMENT - DATA FILES AND SOFTWARE
*  Zope Public License 2.0
*  ACE license
*  Oracle Universal Permissive License (UPL) Version 1.0
*  Open Grid Forum License
*  Google "Additional IP Rights Grant (Patents)" file
*  Historical Permission Notice and Disclaimer

**不建议接纳的第三方依赖的许可证**

原则上，不遵从OSI的开源定义的许可证，不支持商用的许可证，传染性的许可证以及其它包含不合理限制或义务的许可证不建议被接纳，请参考如下不建议接纳的许可证名单：

*  Intel Simplified Software License
*  JSR-275 License
*  Microsoft Limited Public License
*  Amazon Software License (ASL)
*  Java SDK for Satori RTM license
*  Redis Source Available License (RSAL)
*  Booz Allen Public License
*  Confluent Community License Version 1.0
*  Any license including the Commons Clause License Condition v1.0
*  Creative Commons Non-Commercial variants
*  Sun Community Source License 3.0
*  GNU GPL 1, 2, 3
*  GNU Affero GPL 3
*  QPL
*  Sleepycat License
*  Server Side Public License (SSPL) version 1
*  Code Project Open License (CPOL)
*  BSD-4-Clause/BSD-4-Clause (University of California-Specific)
*  Facebook BSD+Patents license
*  NPL 1.0/NPL 1.1
*  The Solipsistic Eclipse Public License
*  The "Don't Be A Dick" Public License
*  JSON License

<!--no_check-->