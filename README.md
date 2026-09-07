# llvm-package

为 Windows 平台上的 Chtholly 开发者提供方便使用的 LLVM 18.1.8 预编译工具链。

该预编译包支持 X86 和 AArch64 两种代码生成目标，基于 C++20、MSVC 以及 RelWithDebInfo 配置构建，并关闭了 LLVM RTTI 和 Assertions。

## 构建配置

* LLVM：18.1.8
* Host：Windows x86_64
* 代码生成目标：X86、AArch64
* C++ 标准：C++20
* 编译器：MSVC
* 构建类型：RelWithDebInfo
* LLVM RTTI：关闭
* LLVM Assertions：关闭
