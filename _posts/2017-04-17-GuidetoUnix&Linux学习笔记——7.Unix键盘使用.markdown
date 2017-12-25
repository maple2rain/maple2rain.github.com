---
layout: post
author: LPF
title: Guide to Unix & Linux学习笔记—— 7.Unix 键盘使用
date: 2017-04-17 19:44:01
updated: 2017-05-19 16:17:32
categories:
- Study
- Computer
- OS
- Linux
- Guide to Unix & Linux 
---
# Teletype和Unix文化

最初的终端键盘由Teletype公司制造，因而终端中是被称为`TTY`。这种习惯被Unix采纳，即便Teletype的时代已经过去了很久，人们仍然使用`tty`作为Unix终端的符号。

- 在Unix系统中，每个终端都有自己的名称。显示自己终端名称的命令是`tty`
- 命令`stty`(set tty)用来显示或改变终端的设置
- 程序`getty`(get tty)用来打开与一个终端的通信，并启动登录进程

----------

# Termcap、Terminfo与curses

为了使程序能够以标准化的方式操作终端，可以使用将所有不同类型的终端的描述收集到一个数据库中，当程序希望向终端发命令时，就可以通过使用数据库中的信息来达成。
第一个这样的系统由伯克利大学的Bill Joy创建，其在封装1BSD的过程中，包含了一个管理各种类型终端的显示屏幕的系统。并在1978年，发布了2BSD，并对该系统进行了完善，将其命名为`Termcap`(Terminal Capabilities)。
要在一个程序中使用Termcap需要很多工作，为了使他方便简单些，另一个伯克利的学生Ken Arnold开发了一个程序界面，即`curses`(Cursor Addressing)。curses用来执行屏幕显示管理所需的所有功能，同时对程序员隐藏细节。
为了有效的工作，Termcap数据库必须包含每台Unix可能使用的终端的每种变体，而且所有这些数据都必须包含在一个单独的文件中。多年以来，随着新型终端的不断出现，Termcap文件变得日益庞大，从而使他难以维护，搜索变慢。
此时，贝尔实验室的程序员增强了curses，并用`Terminfo`(Terminal Information)替换了Termcap。Terminfo将数据存储在一系列文件中，每种终端类型一个文件。这些文件组织在26命名为`a`到`z`的目录中，他们都保存在Terminfo目录下。例如，在Linux中，通用VT100终端的信息就存储在该文件中`/usr/share/terminfo/v/vt100`。
主Terminfo目录的位置根据系统的不同可能有所不同，下面时一些最常使用的目录：

    /usr/share/terminfo/
    /usr/lib/terminfo/
    /usr/share/lib/terminfo/
    /usr/share/misc/terminfo/

Terminfo数据已经进行了编译，这意味着不能直接看到他的内容。因此，必须使用一个特殊的程序`infocmp`来读取数据并翻译成纯文本。
在使用`infocmp`时，只需指定希望查看其信息的终端名称即可，例如，显示VT100终端的Terminfo数据，可以使用以下命令：

```sh
$ infocmp vt100 | less
```
如果要显示当前正在使用的终端的Terminfo数据，可以使用不提供终端名称的`infocmp`命令，如：

```sh
$ infocmp | less
```
----------

# 查看所用终端类型

为了记录正在使用的终端类型，Unix使用了`环境变量`。环境变是一个拥有名称和值的条目，通常由shell和所有运行的程序使用。查看当前使用终端的全局环境变量为`TERM`。
在任何时间，使用命令`echo`后面跟一个`$`(美元符号)和变量的名称都可以显示任何环境变量的值。例如，为了查看`TERM`的值，可以使用以下方式：

```sh
$ echo $TERM
```
该命令将显示当前正在使用的终端的类型。
为了展示所有的环境变量，可以使用下列两种命令：

```sh
$ env
$ printenv
```

`env`表示环境变量(Environment Variables)，`printenv`代表显示环境变量(Print Environment Variables)。

----------

> 提示
因为小写字母容易键入，所以习惯于使用小写字母作为专有名称，包括用户标识、命令和文件。
这一规则的主要例外于环境变量相关。在shell中，环境变量使用大写字母命名，如TERM，这是传统惯例。通过采用这种方式，让人一瞥就知其为环境变量。

----------

# 修饰键：Crtl

`Ctrl`键(Control)用于控制终端操作，经常用于组合键来产生一些指定的效果。一般情况下，可以看到计算机社区上经常使用`Ctrl-A`或`C-a`以及`^A`来表示同时按住`Ctrl`键及`A`键，注意，此时并不需要按下`Shift`键。
对于不同的按键，**stty**会发送不同的信号，即有不同的表示含义，如下表所示：

|按键|含义|解释|
|---|
|C|intr|中断信号，用于终止进程的运行|
|D|eof|输入文件结束符|
|H|erase|删除光标前最后一个键入的字符|
|Q|start|启动屏幕显示键入字符|
|S|stop|停止屏幕显示键入字符|
|U|kill|删除整行|
|W|werase|删除光标前最后一个键入的单词|
|Z|susp|挂起进程，使用fg恢复进程运行|
|\\ |quit|同intr，并生成core dump文件|
 
 
----------

# stty

为了显示系统的键盘映射，可以使用下述命令：

```sh
$ stty -a
```

在`Fedora 25`下，显示的信息如下：

```sh
speed 38400 baud; rows 24; columns 82; line = 0;
intr = ^C; quit = ^\; erase = ^?; kill = ^U; eof = ^D; eol = <undef>;
eol2 = <undef>; swtch = <undef>; start = ^Q; stop = ^S; susp = ^Z; rprnt = ^R;
werase = ^W; lnext = ^V; discard = ^O; min = 1; time = 0;
-parenb -parodd -cmspar cs8 -hupcl -cstopb cread -clocal -crtscts
-ignbrk -brkint -ignpar -parmrk -inpck -istrip -inlcr -igncr icrnl ixon -ixoff
-iuclc -ixany -imaxbel iutf8
opost -olcuc -ocrnl onlcr -onocr -onlret -ofill -ofdel nl0 cr0 tab0 bs0 vt0 ff0
isig icanon iexten echo echoe echok -echonl -noflsh -xcase -tostop -echoprt echoctl
echoke -flusho -extproc
```

----------


## 修改键映射

如果希望修改键映射，可以使用`stty`命令。只需键入`stty`，后面跟着信号的名称，然后是新的键赋值即可。
例如，为了将`kill`键改为`^U`，使用的命令如下：

```sh
$ stty kill ^U
```

注意，上述的`^U`并不是组合键的意思，而是两个单独的字符`^`和`U`。`stty`会自行将其解释为组合键`Ctrl-U`。
严格地将，可以将任意键映射到一个信号上，但这样会带来麻烦，比如将`kill`信号映射到字母`U`上，那么当按下字母`U`时，将会把刚键入的一行字符都删除，不过这样倒是可以作为一个恶作剧！🙂

当然，键盘操作还不只这些，更多高级技巧可以查看[这篇博文](http://maple2rain.leanote.com/post/%E9%94%AE%E7%9B%98%E9%AB%98%E7%BA%A7%E6%93%8D%E4%BD%9C%E6%8A%80%E5%B7%A7)，里面介绍的是终端下采用emacs方式的快捷键，也可以修改为vi的方式，方法是改变环境选项，如下：

```sh
$ set -o vi
```
若想持续有效，还得将其写入`.bashrc`文件。

----------


# 返回和换行

在Unix下，返回信号是`^M`，换行信号是`^J`。
关于返回和换行，这里有一个有意思的事情：

- 返回字符=`^M`
- 换行字符=新行字符=`^J`
- 一般而言，每行文本必须以一个新行字符结束
- 当按下`Return`或`Enter`键时，将发送一个返回字符，Unix自动将返回字符改变为新行字符
- 在终端上显示数据时，每行必须以字符序列`返回+新行`结束。因此，当数据以文件发送到终端显示时，Unix自动将每行末尾的新行字符改变为返回字符+换行字符
- 当无法输入`Enter`或`Return`键时，可以考虑使用`^M`或`^J`键来代替

----------


# Reference

[1] Haeley Hahn. Unix & Linux 大学教程[M]. 张杰良, 译. 北京:清华大学出版社, 2010.1:114-140.