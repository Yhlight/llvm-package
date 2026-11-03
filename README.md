# llvm-package

为 Windows x86_64 上的 Chtholly 开发者提供 LLVM 18.1.8 预编译开发包。

这些包由维护者本机已安装的 LLVM 工具链目录打包而成，用于共享 Chtholly 的开发环境。它们是开发 SDK，包含编译器开发所需的头文件、静态库、CMake package 和命令行工具；不是 Chtholly 编译器发行包，也不包含完整的 MSVC / Windows SDK 开发环境。

下载入口：[LLVM 18.1.8 Prebuilt Packages for Windows](https://github.com/Yhlight/llvm-package/releases/tag/v18.1.8)。

## 版本选择

Chtholly 日常开发优先选择 **X86/AArch64 RelWithDebInfo** 包。全后端 Release 包适用于需要其他 LLVM 代码生成后端的开发工作，构建选项不同，不能仅凭 LLVM 版本相同就将两者视为可无条件替换。

| 配置 | RelWithDebInfo | Release（全部后端） |
| --- | --- | --- |
| LLVM 版本 | 18.1.8 | 18.1.8 |
| 运行宿主 | Windows x86_64 | Windows x86_64 |
| 构建工具链 / C++ 标准 | MSVC / C++20 | MSVC / C++20 |
| LLVM 库 | Static | Static |
| LLVM 代码生成后端 | X86、AArch64 | 见下方完整列表 |
| Assertions | 开启 | 关闭 |
| RTTI | 开启 | **关闭** |
| C++ Exceptions（EH） | 开启 | **关闭** |
| ABI-breaking checks | 开启（1） | **关闭（0）** |

2026-11-03 根据维护者本机两套安装中的 `lib/cmake/llvm/LLVMConfig.cmake` 和 `include/llvm/Config/abi-breaking.h` 核对了版本、配置、后端、assertions、RTTI、EH 和 ABI-breaking checks。此前 README / Release 中将全后端 Release 的 RTTI 写为开启，现予以更正。

Release 包的后端列表：AArch64、AMDGPU、ARM、AVR、BPF、Hexagon、Lanai、LoongArch、Mips、MSP430、NVPTX、PowerPC、RISCV、Sparc、SystemZ、VE、WebAssembly、X86、XCore。

这里的后端指 LLVM 能生成代码的目标。包本身仍运行于 Windows x86_64；包含某个 LLVM 后端不表示 Chtholly 已支持该运行平台。

## 下载文件与 SHA-256

固定发布为 `v18.1.8`。以下校验值来自 GitHub Release asset 的 SHA-256 元数据：

### RelWithDebInfo（推荐用于 Chtholly 开发）

文件：[llvm-18.1.8-host-windows-x86_64-targets-x86_64-aarch64-relwithdebinfo.tar.xz](https://github.com/Yhlight/llvm-package/releases/download/v18.1.8/llvm-18.1.8-host-windows-x86_64-targets-x86_64-aarch64-relwithdebinfo.tar.xz)

```text
9594abdeb7b24a568955c775ff3e806a9c7fc41b06766579555743f595324098
```

### Release（全部后端）

文件：[llvm-18.1.8-host-windows-x86_64-targets-all-release.tar.xz](https://github.com/Yhlight/llvm-package/releases/download/v18.1.8/llvm-18.1.8-host-windows-x86_64-targets-all-release.tar.xz)

```text
96350aac30040cc36f5d3a10f9e3033614b900cd97b587d21a573ef8e1cb153b
```

下载后可在 PowerShell 中核对文件哈希：

```powershell
Get-FileHash -LiteralPath '.\llvm-18.1.8-host-windows-x86_64-targets-x86_64-aarch64-relwithdebinfo.tar.xz' -Algorithm SHA256
```

哈希相同说明下载文件与记录的发布 asset 一致，不替代消费工程的 ABI 兼容性验证。本次修订核对了本机安装元数据及发布信息，没有重新下载、解压或运行发布包。

## 使用

准备 MSVC x64 C++ 开发工具、Windows SDK、CMake 和 Ninja。使用支持 `.tar.xz` 的工具解压，并选择实际包含下列目录的安装根目录；不要假定归档外层目录的名字。

```text
<LLVM 安装根目录>/
├── bin/                   # 包含 clang-cl.exe、lld-link.exe 等工具
├── include/               # LLVM / Clang 开发头文件
└── lib/
    ├── ...                # 开发库
    └── cmake/llvm/         # LLVMConfig.cmake 等 CMake package 文件
```

在已经导入 MSVC x64 开发环境的 PowerShell 中，为 Chtholly 选择该安装：

```powershell
$chthollyLlvmRoot = 'D:\deps\llvm-18.1.8' # 替换为实际解压根目录。
$env:CHTHOLLY_LLVM_DIR = Join-Path $chthollyLlvmRoot 'lib\cmake\llvm'
$env:CHTHOLLY_LLVM_BIN = Join-Path $chthollyLlvmRoot 'bin'
```

具体构建命令按所用 Chtholly 版本的文档执行。一般 CMake 消费工程可通过 `LLVM_DIR` 指向同一 `lib/cmake/llvm`。

## ABI 与来源边界

- LLVM 头文件、库和工具必须来自同一套安装，尤其不能混用两个包的头文件与静态库。
- 消费工程需匹配 MSVC CRT、STL ABI 和 LLVM ABI-breaking checks；启用 assertions 不等于使用 MSVC Debug CRT。
- 两套本机安装的 `LLVMConfig.cmake` 中 `CMAKE_MSVC_RUNTIME_LIBRARY` 均未填值，因此不能仅凭该导出变量断言 `/MD` 或 `/MT`。结合原构建记录和消费工程的兼容性要求核对，不用链接器忽略选项掩盖差异。
- 这些文件共享维护者的已安装环境；当前仓库未提供完整可复现构建配方，不承诺重新构建得到逐字节相同的包。

## 许可证

LLVM 上游使用 Apache-2.0 WITH LLVM-exception，并包含按各自许可分发的第三方内容。请参阅 [LLVM 18.1.8 许可证](https://github.com/llvm/llvm-project/blob/llvmorg-18.1.8/llvm/LICENSE.TXT)及所用组件的许可通知。发布预编译包不会将 LLVM 重新许可为 Chtholly 自身的 MIT OR Apache-2.0。
