# 许可证声明与版权声明规范

## 目的

本规范明确了ArkUI-X的代码贡献者、Committer、PMC成员如何处理Repo及源代码文件的许可与版权声明，包括如下几个部分

**（1）版权声明**

**（2）许可证声明**

**（3）版权和许可证声明的例子**

**（4）第三方开源软件使用声明**

## 范围

本规范仅适用于ArkUI-X社区，不适用于将ArkUI-X应用于个人或企业以开发其它产品的场景，也不适用分发第三方开源软件的场景（该场景参见第三方开源软件引入指导）。

**（1）代码版权声明规范**

开源仓中的文件原则上都应当包含合适的版权声明，开发者在贡献代码时可以在代码中声明代码的版权所有者。

**版权声明形式如下：**

**Copyright (C) [第一次发布年份]-[当前版本发布年份] [版权所有者]**
or
**Copyright (C) [首次发布年份]-[当前版本发布年份] [版权所有者]**

上述版权声明中的年份表示作品对外发布的年份，如果是第一次发布则写发布年份即可，如果不是第一次发布，则写 "第一次发布年份-当前版本发布年份"。也可以选择只表明首次发布年份的形式。

版权所有者是法律实体，可以是个人或者公司，若代表公司贡献代码，版权所有方归属开发者所属公司，版权声明请写公司法律实体名称。

版权声明形式与位置：

版权头：版权声明优先选择在代码文件头部，称为版权头。

可以不进行版权声明的例外场景：

添加版权和许可声明会影响到该文件的功能，如JSON文件因不支持注释，可不添加版权和许可头。

工具生成的文件且包含说明该文件是由工具自动生成的描述信息。

简短的供用户阅读的说明文件，添加版权许可头会影响其可读性和，如README等。

**（2）许可证声明规范**

每个开源代码仓必须有清晰的许可证声明，许可证声明的形式包括许可证头和许可证声明文件。

**许可证声明文件**： 

许可证声明文件是采用单独的Legal Note文件用来声明许可证。

每个开源仓的许可证声明文件必须为纯文本格式，以“LICENSE”命名，不需要带".txt", ".md"等后缀。许可证声明文件里面包含声明的许可证的全文。

许可证声明文件的位置：

许可证声明文件应该放置于代码仓的根目录下。

许可证声明文件的注意事项：

1.  如果开源仓下的不同源码采用多种许可证，建议采用多个许可证声明文件，每个许可证声明文件对应一个许可证。例如，将主许可证描述在以“LICENSE”命名的文件中，其它许可证请以“LICENSE-许可证类型-备注”命名并放置于仓的根目录或该许可证对应源码的根目录，同时在主许可证中声明文件中描述各许可证文件位置及其适用的范围与场景。

2.  每个开源仓的许可证文件必须要涵盖该仓下所有文件，确保各许可证的涵盖范围描述准确、精简，并且不要包含不在本仓发布的其它源代码许可等不必要的信息，比如要单独下载的依赖软件的许可证不要包含在本仓的许可证声明文件中。

3.  如果开源仓在发布时以二进制形式发布，请确保许可证文件位于其发布格式的常规位置，如发布文件夹或压缩包的顶层目录，对于".jar"格式的文件，许可证声明文件可位于META-INF目录。

**许可证头**：

代码头部还可以增加该代码模块适用的许可证信息，称为许可证头。一般来说，许可证头需与其所在开源仓的许可证声明文件中声明的许可证一致。

如果某代码模块采用双重许可证，则许可证头要清晰地说明每个许可证的适用条件，并在许可头中包含每个许可证的文本。

**（3）版权头和许可证声明的例子：**

Copyright [yyyy]- [yyyy] [name of copyright owner]
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
  http://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

**（4）第三方开源软件使用声明**

如果分发源码形式的第三方开源软件，请保持第三方开源软件原始的版权声明和许可证声明，不要修改或删除第三方开源软件中的任何版权声明或许可。如果仅仅对第三方开源软件进行微小修改/添加，请采用与第三方开源软件相同的许可证。

如果分发的二进制文件中包含有第三方开源软件，请提供以“NOTICE”命名的文件，NOTICE文件以纯文本格式描述包含的所有第三方开源软件名称、软件版本、权利人声明、License信息。NOTICE文件通常放置在发布文件夹或压缩包的顶层目录，对于".jar"格式的文件，许可证可位于META-INF目录。