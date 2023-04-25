---
hide: true
title: Mogan 开发笔记
---
> 打算参加墨客实验室的 OSPP 活动，选中 Mogan Draw 这个项目，故开始上手墨干编辑器。

相对 Linux 而言，Windows 上环境的构建可能会比较麻烦。但是由于笔者的其他任务（例如毕业设计、毕业论文、一些游戏）需要 Windows 环境，故花数天时间折腾，成功在 Windows 上运行项目。

如果有条件，请尽可能使用 Linux 构建、开发本项目。

## 编译 & 运行

> 此处感谢 [jingkaimori](https://github.com/jingkaimori) 大佬的指导 orz

1. 准备好 mingw 和 Qt

   > mingw 的版本必须为 8.1.0，Qt 必须为 Qt5
   >

   > 使用更高版本的 mingw 会导致 `libcurl` 的构建失败，用 Qt6 会导致 mogan 本体编译失败
   >
2. 准备好 msys2 构建环境和 xmake 工具
3. 从 GitHub Actions 扒下来这些构建命令，在 Mogan 项目目录运行即可

```
xmake config --yes --verbose --diagnosis --plat=mingw --mingw=/c/ProgramData/chocolatey/lib/mingw/tools/install/mingw64/ --qt=/c/Qt5/5.15.2/mingw81_64/
xmake build --yes --verbose --diagnosis --jobs=4 --all
windeployqt --compiler-runtime ./build/mingw/x86_64/release/ -printsupport
xmake install mogan_install
windeployqt --compiler-runtime ./build/package/bin/ -printsupport
```

首次构建时间可能会比较久。构建完成后，启动 `build/package/bin` 中的 `mogan.exe` 即可。

构建 Windows 版本时遇到任何问题，可发邮件给我。我相信 Windows 构建上的大部分问题我都已经遇到过了...（Windows 真的及其不适合 C/C++开发 qwq

## 项目架构

墨干编辑器基于 Standard S7 Scheme 和 C++ 进行开发。

C++ 是大家的老朋友了，这里不过多介绍。Scheme 语言是 Lisp 的一种实现，非常古老。

Mogan 的项目中，底层的东西使用 C++ 编写。一些规则和配置则是使用 Scheme 描述（据说这是 GNU 的老传统了）。

Scheme 虚拟机对应 Mogan 源码中的 `src/Scheme/S7/s7.h` 和 `src/Scheme/S7/s7.c` 这两个文件（其中 `s7.c` 有恐怖的 9 万 6 千余行代码）。同目录下的 `s7_tm.hpp` 则是 Mogan 与 Scheme 虚拟机的一个接口，其中做了一些基本数据类型的转换（比如 `tmscm_to_bool`、`bool_to_tmscm`）和一些基本功能的封装（例如 `call_scheme`）。开发 Mogan 时，我们不用关注虚拟机部分的源码。

## 一些工具函数

`void *safe_malloc(size_t)` 安全地申请内存。若系统内存不足，则 abort。

`void *fast_new(size_t)` 工程中用于申请内存的函数，在调试模式下会输出一些内存相关的信息。

`tm_new` 一个更安全的 `new`，可以分配内存 & 调用其构造函数。

**入口函数**

Mogan 的入口函数是位于 `src/Texmacs/Texmacs/texmacs.cpp` 中的 `void TeXmacs_main(int argc, char **argv)`。

开始的二百余行代码均为初始化代码，检查了启动参数、环境变量等内容。

此后，入口函数调用了 `gui_open` 启动了 UI 界面。`gui_open` 函数初始化了 `the_gui` 这个全局变量，这也是 Mogan UI 的对象。

最后是一个 code block，启动了 Mogan Server。Mogan 是一个 C/S 模式的程序，Server 负责提供核心逻辑，Client（也就是 `the_gui`）负责渲染内容。

**the_gui**

属于 `qt_gui_rep` 类，是 Mogan UI 全局对象，在 `gui_open` 中被初始化。
