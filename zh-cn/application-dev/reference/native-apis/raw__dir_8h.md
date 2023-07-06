# raw_dir.h


## 概述

提供rawfile目录相关功能。

功能包括遍历和关闭rawfile目录。

**起始版本：**

8

**相关模块：**

[Rawfile](rawfile.md)


## 汇总


### 类型定义

| 类型定义名称 | 描述 |
| -------- | -------- |
| [RawDir](rawfile.md#rawdir) | 提供对rawfile目录的访问 |


### 函数

| 函数名称 | 描述 |
| -------- | -------- |
| [OH_ResourceManager_GetRawFileName](rawfile.md#oh_resourcemanager_getrawfilename) ([RawDir](rawfile.md#rawdir) \*rawDir, int index) | 通过索引获取rawfile文件名称 |
| [OH_ResourceManager_GetRawFileCount](rawfile.md#oh_resourcemanager_getrawfilecount) ([RawDir](rawfile.md#rawdir) \*rawDir) | 获取[RawDir](rawfile.md#rawdir)中的rawfile数量 |
| [OH_ResourceManager_CloseRawDir](rawfile.md#oh_resourcemanager_closerawdir) ([RawDir](rawfile.md#rawdir) \*rawDir) | 关闭已打开的[RawDir](rawfile.md#rawdir)并释放所有相关联资源 |
