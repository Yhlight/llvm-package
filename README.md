# llvm-package

为 Windows 平台上的 Chtholly 开发者提供开箱即用的 LLVM 18.1.8 预编译工具链。

目前提供两个构建版本：

* **RelWithDebInfo 精简版本**：仅构建 X86 和 AArch64 后端，适合 Chtholly 的日常开发与调试。
* **Release 完整版本**：构建 LLVM 支持的全部代码生成后端，适合需要多平台代码生成能力的开发场景。

---

## RelWithDebInfo 版本

该版本基于 MSVC 和 C++20 构建，采用 RelWithDebInfo 构建配置，并仅支持 X86 与 AArch64 两种 LLVM 代码生成目标。

### 构建信息

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

该版本主要面向 Chtholly 在 Windows 平台上的开发与构建需求，在保留调试信息的同时启用了编译优化，适合日常开发、调试及问题排查。

---

## Release 完整版本

该版本基于 MSVC 和 C++20 构建，采用 Release 构建配置，并构建 LLVM 支持的全部代码生成后端。

### 构建信息

* LLVM 版本：18.1.8
* Host 平台：Windows x86_64
* 构建类型：Release
* C++ 标准：C++20
* 编译器：MSVC
* LLVM Assertions：关闭
* LLVM RTTI：启用
* LLVM 库链接模式：Static

### 支持的代码生成目标

* AArch64
* AMDGPU
* ARM
* AVR
* BPF
* Hexagon
* Lanai
* LoongArch
* Mips
* MSP430
* NVPTX
* PowerPC
* RISCV
* Sparc
* SystemZ
* VE
* WebAssembly
* X86
* XCore

该版本提供完整的 LLVM 后端支持，适合需要进行跨平台代码生成、LLVM 后端开发或其他高级 LLVM 开发工作的场景。

---

## 版本选择

| 版本           | RelWithDebInfo | Release            |
| ------------ | -------------- | ------------------ |
| 构建优化         | 开启             | 开启                 |
| 调试信息         | 保留             | 不保留                |
| Assertions   | 启用             | 关闭                 |
| RTTI         | 启用             | 关闭                 |
| LLVM Targets | X86、AArch64    | 全部 Targets         |
| 推荐用途         | Chtholly 日常开发  | 完整 LLVM 开发、多平台代码生成 |

## 软件包内容

两个版本均包含完整的 LLVM 开发所需文件：

```text
llvm/
├── bin/        # LLVM、Clang 等可执行工具
├── include/    # LLVM、Clang 开发头文件
├── lib/        # LLVM、Clang 静态库
└── libexec/    # LLVM 辅助工具
```

## 适用场景

该工具链主要面向 Chtholly 在 Windows 平台上的开发与构建需求，可作为 LLVM/Clang 开发 SDK 使用。
