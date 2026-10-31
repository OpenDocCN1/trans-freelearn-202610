
One of Red's greatest strengths is the ability to easily create **领域特定语言** (**DSLs**) or **方言**。These are small and optimized sub-languages designed for a specific task, such as `parse` for transforming text (see [第 8 章](#755a09a4-dfeb-499d-8f99-11dd0f0b925f.xhtml), *解析数据*), `view` and `draw` for composing graphical user interfaces (see [第 9 章](#97dee83c-19eb-4b99-8e08-720cb23a61ad.xhtml), *组合视觉界面*), and `Red/C3` for blockchain programming (see [第 10 章](#e4243933-9849-4b1f-8935-504ad83a98ec.xhtml), *高级 Red*).

# Red 的主要特性

As a programming language, Red embraces nearly all paradigms:

*   **命令式**: Providing everything needed for controlling code execution and error-handling (see [第 4 章](#d6a19492-2244-47dc-a52e-64b113c7ce0a.xhtml), *代码控制结构* and [第 5 章](#35fe4037-dd49-406f-b90b-217052b1d33f.xhtml), *使用序列和块*)

*   **函数式**: With functions as first class values (see [第 6 章](#5c76066d-61d4-4dbd-b5db-902ae9e9991c.xhtml), *使用函数和对象*)

*   **面向对象**: Complete with inheritance, but not class-based (see [第 6 章](#5c76066d-61d4-4dbd-b5db-902ae9e9991c.xhtml), *使用函数和对象*)

*   **并发**: With async task support and an actor model (from v 0.9.0)

*   **符号式**: Being able to manipulate its own code as if it was plain data (all chapters)

*   **响应式**: Included in the **GUI** (**图形用户界面**) system (see [第 9 章](#97dee83c-19eb-4b99-8e08-720cb23a61ad.xhtml), *组合视觉界面* and [第 10 章](#e4243933-9849-4b1f-8935-504ad83a98ec.xhtml), *高级 Red*)

Aside from REBOL and LISP, Red has taken inspiration from the following:

*   Forth and Logo for its syntax

*   Lua, for its capabilities as an embedded language, and its **JIT** (**即时**) compiler

*   Self, for its prototype object-model

*   Scala, for its type inference engine and compilation

Although it can be statically compiled, Red has a graphical **REPL** console (**读取求值打印循环**) like other dynamic and scripting languages, to experiment with code interactively. We will explore this console in [第 2 章](#e7b0526e-a6dd-4c51-abc8-81b1dfdc8460.xhtml), *设置开发环境*.

In contrast to REBOL, which is an interpreted language only, Red also *compiles to native code* through `Red/System`, thus achieving much better performance, something like Crystal compared to Ruby. Compared to C, at this moment `Red/System` is 2-4x slower.

## 可移植性

Red runs in the Windows, Linux, Android, macOS, and FreeBSD environments in a 32-bit version (the 64-bit version is planned) with cross-compilation capability. Furthermore, it runs on ARMv5 processors and on the Raspberry Pi and Arduino boards.

## 如同瑞士军刀

The versatility of the Red platform makes it suitable for apps on the entire spectrum, such as:

*   High-level scripting (for example, glue code such as Lua)

*   Web apps

*   GUI desktop applications

*   Android apps (from Version 0.7 onward)

*   2D games

*   Data processing

*   Grammar parsing and constructing specialized languages

*   Network/system programming, IoT devices programming, robotics

## 摘要

In this chapter, we looked at Red's main characteristics, and examined its place among other programming languages. In the following chapter, we will set up an environment to start up our development with Red.

## 问题

1.  Why does Red have a `Red/System` component?

2.  What is a full-stack language, and why is Red an example of that?

3.  What do the the Red and LISP languages have in common?

4.  Give three ways in which Red reduces software complexity.

5.  What is a dialect in Red? Give some examples.

6.  Why is Red faster than REBOL?

7.  Name some of the environments in which Red can run.

# 设置开发环境

Now we are going to set the stage for our future Red development. We'll get Red on to your computer, and then immediately start working in the Red console to get a feel for the environment. Then, we'll create our first script, run it in the console, and then compile and execute it. We will also discuss how Red compiles and what the different compilation options are. Finally, we'll show some coding environments to craft your Red code with ease.

We will cover the following topics:

*   Installing Red

*   Working interactively in the Red console

*   Running and compiling your first script

*   Other compilation options

*   Red's compilation model

*   Development environments for writing Red

## 技术要求

We'll download the Red toolchain from here: [http://www.red-lang.org/p/download.html](http://www.red-lang.org/p/download.html)

You'll find the code for this chapter here: [https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter02](https://github.com/PacktPublishing/Learn-Red-Fundamentals-of-Red/tree/master/Chapter02)

## 安装 Red

To get Red on your machine, simply download and save the latest stable version that corresponds to your **操作系统** (**OS**) from here: [http://www.red-lang.org/p/download.html](http://www.red-lang.org/p/download.html). The download is named `red-num`  (on Windows, the file also has an `.exe` extension), where `num` is the version number, such as 063. This small file (around 1 MB) contains the full Red toolchain, runtime library, and console source code, truly batteries included! It is that small because no external dependencies other than the OS are included.

On the downloads page, you can also find the nightly automated build and the latest source code from the `master` branch. If you want to work with a Red version that has the latest functionalities, download the nightly master build. For regular development, you better stick with the last stable version. 

Red's runtime library is written as a mix of Red and `Red/System`. At this time, the Red toolchain (compilers, linkers, packagers) still depends heavily on REBOL 2. For example, the current compilers for Red and `Red/System` are written in REBOL 2. The goal for version 1.0 or beyond is to remove this REBOL dependency, so that Red is completely self-hosted.

### 在 Windows 上安装 Red

Save the file in a suitable folder (it can be installed anywhere), let's say `c:\\red`. For convenience, rename the file to `red.exe`. Now double-click this file—this will then automatically build the Red GUI-console, taking a few moments.

The `gui-console-yyyy-m-dd-nnnnn.exe` file (`yyyy-m-dd` is a build date and `nnnnn` a build number) is cached on disk in the `c:\\ProgramData\\Red` folder. Then the Red GUI console appears, waiting for your input at the `\>>` prompt:

![](assets/081f23f3-b06c-40b8-8997-39cd7948107a.png)About the version  

The first line shows that the current Red version at the time of writing is 0.6.3. The stable Red 0.6.3 version has only two menu-items—File and Options. This screenshot was built from the master branch in mid-April 2018, which already shows the 0.6.4 layout. You'll probably see a higher version number in your download. 

This Red console is completely written in Red.

Next time you start `red.exe`, the built-in REPL GUI console will open immediately. To make the Red toolchain available to your entire system, add the installation folder (here `c:\\red`) to the `Path` environment variables using Configuration, System, Advanced Configuration, Environment Variables.

Here are some useful remarks to help you out in certain cases:
