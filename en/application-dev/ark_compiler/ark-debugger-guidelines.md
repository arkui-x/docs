# ArkCompiler Debugger
ArkCompiler Debugger allows you to debug ArkUI-X applications with the following features: stepping, breakpoints, watch variables, and expressions. It is currently available on Chrome and will be extended to HUAWEI DevEco Studio in the near future.

## Debugging Applications Running on Android
1. Launch the application.
2. Obtain the port number of the debugger socket on the device.

   ```
       adb shell "netstat -anp | grep PandaDebugger"
   ```

3. Map the port on the device to the host for communication. *$port* is the port number obtained in the previous command.

   ```
       adb forward tcp:15037 localabstract:{$port}PandaDebugger
   ```

4. Access [DevTools](devtools://devtools/bundled/inspector.html?ws=//127.0.0.1:15037): <devtools://devtools/bundled/inspector.html?ws=//127.0.0.1:15037> on Chrome for debugging.

5. To restart the application, remove the previous port mapping or map a new port on the host.

   ```
       adb forward --remove tcp:15037 --- Cancel the previous port mapping.
       adb forward tcp:{$new_host_port} localabstract:{$device_port}PandaDebugger --- Map a new port.
   ```


## Debugging Applications Running on iOS
1. Install usbmux on macOS for port mapping.

   ```
       brew install usbmuxd
   ```

2. Map the port on the device to the host.

   ```
       iproxy 15037 9230
   ```

3. Access [DevTools](devtools://devtools/bundled/inspector.html?ws=//127.0.0.1:15037): <devtools://devtools/bundled/inspector.html?ws=//127.0.0.1:15037> on Chrome for debugging.