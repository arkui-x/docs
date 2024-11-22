# 跨平台框架规格说明

**规格1**：当前支持Stage模型，FA模型及类web范式出于优化SDK体积考虑已经不再支持；

**规格2**：移动端当前支持arm64和arm32位架构；

**规格3**：Android平台最低支持版本为API26+（Android8.0及以上），iOS最低版本为iOS10+；

**规格4**：Android平台当前既支持Activity级别的跨平台，也支持Fragment级别的跨平台；

**规格5**：HarmonyOS Next应用中跨平台模块的bundleName必须和Android、iOS应用包名一致；

**规格6**：ArkUI-X提供的Bridge通信机制，要求拉起对应跨平台界面时才可以，不支持在原生界面直接和TS侧通信；

**规格7**：ArkUI-X框架支持鸿蒙原生应用hap、hsp类型包结构并遵循鸿蒙应用开发规范，其中hsp不可单独跨平台，必须有hap类型提供UIAbility才可以；

**规格8**：支持一个鸿蒙原生应用工程中仅有部分module跨平台，module类型为hap或hsp；

**规格9**：部署跨平台模块时moduleName必须应用内唯一，不允许重名，无论是应用内预置还是动态加载都必须遵循arkui-x下放置module的目录层级结构；

**规格10**：当前不支持一个module即随应用预置又在沙箱内动态加载，即当前module加载优先级为应用预置>沙箱目录，只要在预置目录找到对应module就会加载。

