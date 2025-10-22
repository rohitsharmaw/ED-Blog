---
title: 快速配置 VSCode
date: 2025-10-21 19:11:47
tags: ['C++', 'VSCode', '配置']
categories: ['工具', '分享']
katex: true
author: ED_Builder
---

你甚至只需要执行脚本和编辑输入文件

<!-- more -->

## 前言
众所周知，[Visual Studio Code](https://code.visualstudio.com) 是一款非常好用的编辑器。  
但是在机房，你不得不去用 Dev-C++，如果你希望在机房也能用上很方便写题的 VSCode，本文将会对你有帮助。
## 观前须知
本文仅针对 Windows 设备进行配置，对于 Linux 设备应该差不多（在安装和脚本编写会有差异），MacOS 我没用过，不知道……
## 安装需要的软件
### Visual Studio Code
[Visual Studio Code](https://code.visualstudio.com/download) 支持 Windows10 及以上的系统版本，在前面的链接中就可以选择下载。

如果你的系统在 Windows7 及以下，你只能下载 `v1.70.2` 版本（这是最后的可用版本，再高就直接无法运行了）  
如果你是机房用户，推荐你下载压缩包存档版本，[这里](https://update.code.visualstudio.com/1.70.2/win32-x64-user/stable) 给出 Windows7 x64 的压缩包版本，解压到合适的位置就可以直接点击 `Code.exe` 启动了
### 编译器
这一步分情况讨论，一般情况下机房安装的 Dev-C++ 都会自带编译器的，也支持 C++14 标准（起码我的机房支持）；如果你没有编译器就需要其他方法  
最后还要将编译器添加到环境变量（PATH）
#### 检查现有编译器
你可以先在终端使用 `g++ --version` 检查编译器版本，如果有正常的版本号输出就可以直接跳过编译器部分  
下面是我在 Codespaces 上给出的正常信息
```
@rohitsharmaw ➜ /workspaces/ED-Blog (main) $ g++ --version
g++ (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0
Copyright (C) 2023 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

一般情况下，你的机房安装 Dev-C++ 时安装路径都可能是默认的 `C:\Program Files (x86)\Dev-Cpp`，在你的 Dev-C++ 安装目录中，打开 `MinGW64/bin`，这里是编译器存放的二进制文件，你可以应该能找到 `g++.exe` `gcc.exe` `gdb.exe` 这些文件  
或者你已经搞到了编译器，你只需要确定可执行文件的绝对路径就好了

对于 Linux 系统，你可以直接用 `g++ --version` 检查是否存在。

如果你找到了编译器，可以跳过下面的安装部分，进行一个编译小测试
#### 安装编译器
首先是最简单的 Linux 环境下安装，在终端执行
```sh
sudo apt update
sudo apt upgrade
```
这会更新软件列表和现有软件；然后
```sh
sudo apt install g++
```
安装 G++，等待安装完成就好了。

如果你是 Windows 用户，可以先参考 [VSCode 官方文档的安装说明](https://code.visualstudio.com/docs/cpp/config-mingw#_installing-the-mingww64-toolchain)（需要系统版本是 Windows8.1 及以上）。或者尝试我找到的编译器：[Link](https://wwnh.lanzoum.com/iNtFo38zuhhg) ，密码 `edblog`，需要你能解压 `.7z` 格式的压缩包
#### 添加环境变量
如果你是 Linux 用户请跳过。

Windows 用户请打开控制面板 > 用户帐户和家庭安全 > 用户帐户，左边栏 更改我的环境变量。  
选中一个已有的变量，点击“编辑”，修改变量值。对于 PATH 变量，点击“编辑”后，你可以直接添加或修改路径。在 Windows10 及以上版本中，PATH 变量的编辑界面是图形化的；而在旧版本中，多个路径之间需要用分号 `;` 分隔  
反正这个步骤网络上已经有很详细的步骤了，或者你可以问问 AI

你要添加的环境变量应该是编译器可执行文件所在的文件夹的绝对路径，例如 `C:\Program Files (x86)\Dev-Cpp\MinGW64\bin`
#### 测试编译器
打开一个文件夹，自己写个程序运行一下，保存后打开终端，使用下面的指令执行编译源文件操作：
```sh
g++ SourceFileName.cpp -std=c++14 -O2 -o ExecutableFileName -Wall
```
解释一下每个编译参数：

- 你要把 `SourceFileName.cpp` 替换成你的源文件的文件名，比如你在这个文件夹里放了 `std.cpp`，那么你应该替换成 `std.cpp`
- `-std=c++14` 用于设定编译的 C++ 标准，OI 上使用 C++14 标准，如果你希望使用其他的标准只需要修改后面的数字就好了，比如 `-std=c++17`
- `-O2` 不用说了吧…… O2 时间优化
- `-o ExecutableFileName` 需要你把 `ExecutableFileName` 替换为你希望生成的可执行文件的文件名，比如我想生成 `code.exe`（Windows 环境）或 `code`（Linux 环境），那么你应该替换成 `-o code`
- `-Wall` 是可选的显示警告参数，开启后会显示所有常见的编译警告，可以查错

例如我的源文件是 `std.cpp`，使用 C++14 标准，开启 O2 优化，显示全部警告，输出到 `program.exe`，那我应该使用
```sh
g++ std.cpp -std=c++14 -O2 -Wall -o program`
```

然后你就可以运行你的可执行文件了！
## 构建工作环境
### 工作文件夹
你需要创建一个特定的专门用于写程序的文件夹，然后创建如下文件
- `std.cpp`：这是你的源代码
- `r.bat` 或 `r.sh`：用于直接运行可执行文件
- `c.bat` 或 `c.sh`：用于编译并运行可执行文件，遇到编译错误将会终止执行
- `in` 和 `out`：其实你要不要后缀名都无所谓，因为你只是用来放输入文件和查看输出文件

其实你要换文件名也完全可以，但别忘记修改脚本源码里面的文件名
### 脚本编写
#### 编译运行
上文已经让你配置好了编译器的 PATH 了，下面我们进行编译，并通过管道传输输入和输出数据

c.bat
```bat
@echo off
echo "Compilation Started."
g++ std.cpp -std=c++14 -O2 -Wall -o std
if %errorlevel% neq 0 (
    echo "Compilation Failed!"
    exit /b %errorlevel%
)
echo "Compilation Completed."
std < in > out
echo "Execution Completed."
```

c.sh
```sh
#!/bin/bash
echo "Compilation Started."
g++ std.cpp -std=c++14 -O2 -Wall -o std
if [ $? -ne 0 ]; then
    echo "Compilation Failed!"
    exit $?
fi
echo "Compilation Completed."
./std < in > out
echo "Execution Completed."
```
#### 直接运行
用于测多组大样例

r.bat
```bat
@echo off
std < in > out
echo "Execution Completed."
```

r.sh
```sh
#!/bin/bash
./std < in > out
echo "Execution Completed."
```
