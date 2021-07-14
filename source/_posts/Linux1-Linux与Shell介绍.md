---
title: Linux1-Linux与Shell介绍
date: 2021-07-05  22:12
tags: Linux
categories: Linux学习
---

## 操作系统发展史

回顾一下操作系统的发展史，可以分为四大时代

- 第一个是OS时代，这个时候操作系统才刚刚成型，1973 年由贝尔实验室开发的UNIX 系统，以及 1982 年与 1991年在 UNIX 系统基础上进行扩展定制的若干变种

- 第二个时代是 PC 时代，PC 时代崛起于 1975 年，当年乔布斯开发了 Apple 系统，随后 1980 年，比尔盖茨开发了 DOS 系统，从这时起更多的人开始接触操作系统，个人计算机得以普及

- 第三个时代是 GUI 时代，GUI 时代的代表作是 1979 年乔布斯开发的 Mac 系统与 1990 年比尔盖茨开发的 Windows 系统，以及 1994 年的 Linux 系统，这三个系统影响了整个时代，一直到现在仍被广泛使用

- 第四个时代是移动 OS 时代，随着移动互联网的发展，移动 OS 也变得越来越重要，在移动 OS 时代，最知名的是 Google 的 Android 系统，以及乔布斯的 iOS 系统

## Bash介绍

Bash 是 Unix 系统和 Linux 系统的一种 Shell（命令行环境），是目前绝大多数 Linux 发行版的默认 Shell。

### Shell的含义

学习 Bash，首先需要理解 Shell 是什么。Shell 这个单词的原意是“外壳”，跟 kernel（内核）相对应，比喻内核外面的一层，即用户跟内核交互的对话界面。

具体来说，Shell 这个词有多种含义。

首先，Shell 是一个程序，提供一个与用户对话的环境。这个环境只有一个命令提示符，让用户从键盘输入命令，所以又称为命令行环境（command line interface，简写为 CLI）。Shell 接收到用户输入的命令，将命令送入操作系统执行，并将结果返回给用户。本书中，除非特别指明，Shell 指的就是命令行环境。

其次，Shell 是一个命令解释器，解释用户输入的命令。它支持变量、条件判断、循环操作等语法，所以用户可以用 Shell 命令写出各种小程序，又称为脚本（script）。这些脚本都通过 Shell 的解释执行，而不通过编译。

最后，Shell 是一个工具箱，提供了各种小工具，供用户方便地使用操作系统的功能。

### Shell的种类

在 Linux 系统中你可以通过 cat 指令来查看 etc/ 下的 shells，可以看到本地支持的 Shell 种类非常多，常见的有 bash、csh、ksh、sh，等等。其中，sh 是 Bash 的早期形态，因为 sh 不是 GNU 项目，所以后期又开发了 Bash。

在 Windows 系统中，是没有 Shell 环境的，Windows 下的 Shell 其实叫作 command，现在升级为 PowerShell，但是 Windows 指令与 Linux 系统并不兼容，因为它本身不是从 Linux/Unix 系统衍生出来的，所以导致 Windows 与目前的OS，如：Mac、Linux、Android、iOS 的命令不兼容。为了解决这个问题，在 Windows 中你可以使用 Git bash，以及 Cygwin 来模拟 Shell 环境。

如果你的系统是 Mac，那么恭喜你，Mac 系统自带了 Terminal，你还可以安装  iTerm2，它们都是标准的 Shell 环境。在 Linux 环境下，建议你使用 Bash，Bash 是目前行业内使用最广泛的 Shell 环境，在 Windows 环境下，建议你使用 Git bash，它几乎包含了 Linux 常用的全部指令。

### 命令行环境

#### 终端模拟器

如果是不带有图形环境的 Linux 系统（比如专用于服务器的系统），启动后就直接是命令行环境。

不过，现在大部分的 Linux 发行版，尤其是针对普通用户的发行版，都是图形环境。用户登录系统后，自动进入图形环境，需要自己启动终端模拟器，才能进入命令行环境。

所谓“终端模拟器”（terminal emulator）就是一个模拟命令行窗口的程序，让用户在一个窗口中使用命令行环境，并且提供各种附加功能，比如调整颜色、字体大小、行距等等。

不同 Linux 发行版（准确地说是不同的桌面环境）带有的终端程序是不一样的，比如 KDE 桌面环境的终端程序是 konsole，Gnome 桌面环境的终端程序是 gnome-terminal，用户也可以安装第三方的终端程序。所有终端程序，尽管名字不同，基本功能都是一样的，就是让用户可以进入命令行环境，使用 Shell。

#### 命令行提示符

进入命令行环境以后，用户会看到 Shell 的提示符。提示符往往是一串前缀，最后以一个美元符号`$`结尾，用户可以在这个符号后面输入各种命令

![image-20210705164233888](https://img.rockche.cn//image-20210705164233888.png)

#### 进入和退出方法

进入命令行环境以后，一般就已经打开 Bash 了。如果你的 Shell 不是 Bash，可以输入`bash`命令启动 Bash。

```bash
$ bash
```

退出 Bash 环境，可以使用`exit`命令，也可以同时按下`Ctrl + d`。

```bash
$ exit
```

### 第一行命令

我自己的电脑是Mac系统，并且安装了iTeam，输入`echo hello shell` ，显示如下

![image-20210705165000639](https://img.rockche.cn//image-20210705165000639.png)



## 整理参考

[网道（WangDoc.com）](https://wangdoc.com/bash/intro.html)

[测试开发核心技术 46 讲](https://kaiwu.lagou.com/course/courseInfo.htm?courseId=17#/detail/pc?id=319)

