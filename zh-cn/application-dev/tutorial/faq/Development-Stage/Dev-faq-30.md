## ArkUI-X开启二进制裁剪（ace_engine_enable_reduce_eh_frame）后如何获取和解析native崩溃堆栈

### 背景与默认行为

ArkUI-X提供Release版本二进制裁剪能力（编译开关`ace_engine_enable_reduce_eh_frame`），通过删除栈展开表索引（`.eh_frame_hdr`）与mini符号表（`.gnu_debugdata`）、并大幅压缩栈展开表（`.eh_frame`）来减小动态库体积。

**该开关默认关闭**。默认情况下，动态库保留栈展开表（Android平台还会在发布库中嵌入`.gnu_debugdata` mini符号表），系统崩溃报告与性能分析工具可正常回溯调用栈，无需开发者做任何额外处理。

只有在开发者主动开启裁剪后，堆栈获取方式才会发生变化，此时请按照本文后续章节操作。

### 建议开启裁剪的场景

开启裁剪会以牺牲堆栈可读性为代价换取体积收益，建议满足以下全部条件时再开启：

- 应用对安装包/动态库体积敏感，确有减小体积的诉求；
- 具备符号解析能力，即能够获取与所使用SDK版本对应的带符号动态库（见[获取带符号动态库的方式](#获取带符号动态库的方式)）。

不满足条件时，建议保持默认（不开启裁剪）。

### 编译开关

通过`--gn-args`传入该开关，Android与iOS平台使用方式一致（将`--target-os`替换为对应平台即可）：

```
# 关闭裁剪（默认，可不显式传入）
./build.sh --product-name arkui-x --target-os android --gn-args ace_engine_enable_reduce_eh_frame=false

# 开启裁剪
./build.sh --product-name arkui-x --target-os android --gn-args ace_engine_enable_reduce_eh_frame=true
```

完整的编译流程与各参数说明，参见[ArkUI-X编译指导](../../../../framework-dev/quick-start/start-with-build.md)。

### 开启裁剪前后的能力对比

| 能力 | 默认（未开启裁剪） | 开启裁剪后 |
| --- | --- | --- |
| 系统崩溃报告调用栈 | 完整链路 | 断裂，可用FP堆栈代替 |
| 性能分析工具调用栈 | 完整 | 断裂，通常仅剩栈顶一帧 |
| 动态库体积 | 较大 | 变小（去掉了栈展开表索引与mini符号表，并压缩了栈展开表） |

### 开启裁剪后C++崩溃堆栈的抓取方式

ArkUI-X引擎（`libarkui_android.so`/`libarkui_ios`）在崩溃时会自动使用FP寄存器。该能力由引擎内建的崩溃处理器提供，**与裁剪开关无关，未开启裁剪时同样生效**；区别在于：未开启裁剪时，系统崩溃报告本身可完整回溯调用栈，FP堆栈是补充信息；开启裁剪后，系统崩溃报告无法再完整回溯ArkUI-X动态库内的调用栈，FP堆栈成为获取native调用栈的主要途径。

**日志格式**

崩溃时输出的日志格式如下（示例）：

```
================ ArkUI-x native crash ================
Fatal signal 11 (SIGSEGV), si_code 1, fault addr 0x16b244458
Backtrace (frame-pointer chain):
  #0 pc=0x79463d2b40 /data/app/.../lib/arm64/libarkui_android.so +0x1d2b40
  #1 pc=0x79463d4068 /data/app/.../lib/arm64/libarkui_android.so +0x1d4068
  #2 pc=0x79463d5a10 /data/app/.../lib/arm64/libarkui_android.so +0x1d5a10
======================================================
```

其中：

- `#N pc=0x...`为该帧的运行时虚拟地址。
- `+0x...`为该帧在动态库内的偏移，可直接用于符号解析。

**抓取方式**

- **Android**：FP堆栈输出到系统日志，可通过以下命令查看：

```
adb logcat -s ArkUI-x:F
```

- **iOS**：FP堆栈在Xcode调试控制台输出。

### 函数名与代码行号的解析方式

将偏移量还原为函数名与代码行号，需要使用带符号（unstripped）的动态库。随SDK发布的动态库为strip后的版本，无法直接解析出文件行号。带符号动态库的获取方式见[获取带符号动态库的方式](#获取带符号动态库的方式)。

**Android**

使用日志中`+0x`后面的偏移量（示例中的`0x1d2b40`），配合`llvm-symbolizer`或`addr2line`：

```
llvm-symbolizer --obj=libarkui_android.so 0x1d2b40 0x1d4068 0x1d5a10
```

```
addr2line -f -C -e libarkui_android.so 0x1d2b40 0x1d4068 0x1d5a10
```

`llvm-symbolizer`与`addr2line`可在LLVM官网获取，也可以直接使用Android应用开发NDK自带的版本（位于NDK目录`toolchains/llvm/prebuilt/<主机平台>/bin/`下）。

**iOS**

使用日志中`+0x`后面的值，配合`atos`：

```
atos -arch arm64 -o libarkui_ios 0x1d2b40
```

### 获取带符号动态库的方式

- **ArkUI-X引擎库（源码编译场景）**：构建产物`lib.unstripped/`目录下的动态库带有完整调试信息（体积远大于发布库，请勿随应用分发），与最终发布库同版本、同编译参数。
- **应用自身的native库**：崩溃调用栈中也可能包含应用工程自身编译的native库，其带符号二进制可在应用工程内查找，解析方法与引擎库一致，命令同上。

### 限制条件

- FP链回溯仅在arm64（aarch64）架构上实现；
- FP堆栈最多输出64个栈帧；
- JS/JIT帧以及编译时省略FP的第三方库帧会导致调用链截断，FP堆栈覆盖的是崩溃点附近的native C++帧，不保证完整调用栈；
- 多个线程同时崩溃时，仅第一个崩溃线程输出FP堆栈；
- 开启裁剪后，Android系统崩溃报告中来自ArkUI-X动态库的帧可能不完整，分析问题时以`ArkUI-x` tag的FP堆栈日志为准。
