# llvm-package

为 Windows 平台上的 Chtholly 开发者提供开箱即用的 LLVM 18.1.8 预编译工具链。

该预编译包基于 MSVC 和 C++20 构建，采用 RelWithDebInfo 构建配置，并支持 X86 与 AArch64 两种 LLVM 代码生成目标。

## 构建信息

* LLVM 版本：18.1.8
* Host 平台：Windows x86_64
* 构建类型：RelWithDebInfo
* C++ 标准：C++20
* 编译器：MSVC
* 代码生成目标：
  * X86
  * AArch64
* LLVM Assertions：启用
* LLVM RTTI：启用
* LLVM 库链接模式：Static

该工具链主要面向 Chtholly 在 Windows 平台上的开发与构建需求，包含 LLVM 开发所需的头文件、静态库及相关工具。
