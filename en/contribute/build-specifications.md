# ArkUI-X Build Specifications

## Purpose and Scope

The ArkUI-X build specifications provide guidance for you to compile and modify build-related source code, scripts, and tools in a standard manner to ensure that the build process is correct, reliable, repeatable, and efficient.

## Definition of Terms

### Build framework

The ArkUI-X build system is a group of software used to build ArkUI-X cross-platform projects. It consists of module build templates, configuration frameworks, and release frameworks.

The ArkUI-X build system uses Generate Ninja (GN) to describe the build process and dependencies, and Python to execute specific build targets.

This document describes the compilation requirements of GN.

### GN

GN: an open-source build tool that uses .gn files to define and describe the build process and generates Ninja build files.

### Ninja

Ninja: an open-source build execution tool that efficiently executes the build process list described in the Ninja file.

## GN Compilation Specifications

### Rule 1.1 

**Do Not**: Use GN to invoke external build tools to build software modules.
**Counterexample**: In GN, use **action** to invoke **automake** and **Make** to build third-party components.
**Requirement**: Port external components to the GN build mode to avoid unnecessary dependencies on the environment during the build and obtain common capabilities, such as compiler security options and AddressSanitizer (ASan), provided by the build framework.
**Exception**: The Linux kernel build framework builds user-mode programs. The kernel can be independently built outside the build framework. It is acceptable that some platforms use GN to include the kernel build in the build process to deliver one-click builds.

### Rule 1.2

**Do Not**: Add compiler security options that have been added to the build system to the GN file of the module.
**Counterexample**: Add **-fstack-protector-strong** to the GN file of the module.
**Requirement**: The default options that have been added globally should not be added again to meet internal and external rules.

| Option      | Parameter                | Default Value|
| -------------- | ------------------------ | ------ |
| Stack protection        | -fstack-protector-strong | On    |
| Fortify Source | -D_FORTIFY_SOURCE=2 -O2  | On    |

### Rule 1.3

**Do Not**: Add compilation options that are opposite to the default build options to GN.
**Counterexample**: Add **-wno-unused** to a module to clear build alarms.
**Requirement**: The default build options represent the default capabilities of the build system. If your module needs to remove some default build options, there must be sufficient reasons.
**Exception**: When porting or using a third-party component, you can overwrite the default build options based on the component requirements.

### Rule 2.1

**Requirement**: Use **gn format** to format GN files to meet the format and typesetting requirements.

### Rule 2.2

**Suggestion**: Use Python instead of shell to compile **action**.
**Note**: The Python environment is easier to keep code unified and can run on multiple operating systems. It also provides better scalability, readability, and testability.

### Rule 2.3
**Rule**: Do not modify the content in the source code directory during the execution of GN and Ninja.
**Note**: The forbidden operations include but are not limited to installing patches for, copying files to, performing build tasks in, and generating intermediate files in the source code directory.

### Rule 2.4
**Rule**: Set the encoding format of the build script to UTF-8 and the newline character to UNIX format.
**Counterexample**: After a script is compiled on Windows, Chinese comments are used and saved as local codes.
