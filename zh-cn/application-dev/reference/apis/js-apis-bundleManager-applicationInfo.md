# ApplicationInfo

> **说明：**
> 本模块首批接口从API version 9 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## ApplicationInfo

| 名称                       | 类型                                                         | 可读 | 可写 | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ | ---- | ---- | ------------------------------------------------------------ |
| name                       | string                                                       | 是   | 否   | 应用程序的名称。                                                 |
| description                | string                                                       | 是   | 否   | 标识应用的描述信息，使用示例："description": $string: mainability_description"。                                                 |
| descriptionId              | number                                                       | 是   | 否   | 标识应用的描述信息的资源id。                                               |
| label                      | string                                                       | 是   | 否   | 标识应用的名称，使用示例："label": "$string: mainability_description"。|
| labelId                    | number                                                       | 是   | 否   | 标识应用名称的资源id。                                               |
| icon                       | string                                                       | 是   | 否   | 应用程序的图标，使用示例："icon": "$media:icon"。                                                 |
| iconId                     | number                                                       | 是   | 否   | 应用程序图标的资源id。                                               |
| codePath                   | string                                                       | 是   | 否   | 应用程序的安装目录。                                             |