# 方舟调试器简介
方舟调试器(ArkCompiler Debugger)为开发者提供了ArkUI-X应用程序调试的工具，目前可在Chrome上进行调试，后续将支持在HUAWEI DevEco Studio上进行调试，其功能包括单步调试、断点调试、Watch变量及表达式。

## 调试运行在安卓系统上的应用
- 启动应用
- 获取设备端debugger socket的端口号
```
    adb shell "netstat -anp | grep PandaDebugger"
```
- 将设备端的端口转发到host端以进行通信, 此处$port为上条指令获得的端口号
```
    adb forward tcp:15037 localabstract:{$port}PandaDebugger
```
- 通过Chrome devtool进行调试，在Chrome上访问[devtool](devtools://devtools/bundled/inspector.html?ws=//127.0.0.1:15037): <devtools://devtools/bundled/inspector.html?ws=//127.0.0.1:15037>

- 如要重启应用需取消之前adb的端口映射，或映射host端的新端口
```
    adb forward --remove tcp:15037 ---取消之前的映射
    adb forward tcp:{$new_host_port} localabstract:{$device_port}PandaDebugger ---映射新的端口
```


## 调试运行在IOS系统上的应用
- MacOS安装usbmux应用以进行端口映射
```
    brew install usbmuxd
```
- 将设备端的端口映射到host端
```
    iproxy 15037 9230
```
- 通过Chrome devtool进行调试，在Chrome上访问[devtool](devtools://devtools/bundled/inspector.html?ws=//127.0.0.1:15037): <devtools://devtools/bundled/inspector.html?ws=//127.0.0.1:15037>