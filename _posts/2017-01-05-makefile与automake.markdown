---
layout: post
author: LPF
title: makefile与automake
date: 2017-01-05 16:28:00
updated: 2017-03-23 19:18:28
tags:
- Linux
categories:
- Study
- Computer
- OS
- Linux
- Tools
---
# 1 make命令好处

* 简化编译时所需要执行的命令
* 若在编译完成时，修改了某个源码的文件，则**make**仅会针对被修改了的文件进行编译，其他的目标文件并不会被更改
* 最后可以依照相依性来更新(update)执行文件
* 约定：将makefile文件中第一个目标定义为all，再列出其他从属目标，否则make命令将只创建他在文件makefile中找到的第一个目标

# 2 makefile的基本语法与变量

## 2.1 基本规则
    目标(target)：目标文件1 目标文件2 ...
    <tab>gcc -o 欲创建的可执行文件 目标文件1 目标文件2 ...

* 以'#'开头表示为批注
* 命令行必须要以**tab**键作为开头才行
* 目标(target)与目标文件之间以"**:**"隔开
* 例子
```c++
main: main.o 
    gcc -o main main.o
clean:
    rm -f main.o
```
上例中，target(main)由目标文件main.o创建，target(clean)负责删除main.o。
若想执行任何一个目标，需执行"**make target**"。其中target可多选，以**空格**分开。

## 2.2 使用变量
由于makefile中重复的数据比较多，因此可以用变量来进行简化。
### 2.2.1 变量语法
* 变量与变量内容以 '=' 隔开，同时两边可以具有空格
* 变量左边不可以有<tab>，因为<tab>表示该行为命令
* 变量与变量内容在'='两边不能具有':'
* 在习惯上，变量以**大写字母**为主
* 运用变量时，以`${变量}`或`(变量)`使用
* 在该shell的环境变量时可以被套用的
* 在命令行模式也可以定义变量
* 可以使用"**$@**"代替目标
* 例子
```c++
LIB = -lm
OBJS = main.o
WA = -Wall
main: ${OBJS}
    gcc -o $@(本来应该是main) ${OBJS} ${LIB}
clean:
    rm -f ${OBJS}
```
### 2.2.2 环境变量优先级
1. make命令后面加上的环境变量
    
    1.1 在make前面加上的环境变量如果在makefile里面有定义，则以makefile里面为主

2. makefile里面指定的环境变量
3. shell原本具有的环境变量

# 3 特殊符号

- 当使用shell命令时，需以**@**作为命令开头，并在每一行代码的后面加上反斜杠**\**以表明是在同一行。
- 减号**-**告诉make命令忽略所有的错误，如**-rm a**，当文件a不存在时，也不会出现错误，继续执行。

# 4 后缀和模式规则
通过定义一条通用规则，可以是带有旧后缀名的文件生成带有新后缀名的文件，格式如下：

```
.<old_suffix>.<new_suffix>:
%.<old_suffix>: %<new_suffix>
```

举个例子，设现有三个文件`main.c`，`sayhello.c`，`sayworld.c`。其中`main.c`中含有`main`函数，而其他两个文件含有辅助工具函数，且在`main`函数中使用了该工具函数，即`main`依赖于其他两个文件，故可如此编写`makefile`：

```makefile
main: sayhello.o sayworld.o # main依赖于后者

.SUFFIXES: .o .c 后缀声明

sayhello.o: sayhello.c # 依赖
sayworld.o: sayworld.c # 依赖

.c.o: # 将后缀.c转换为.o
    gcc -c $< # $<表示名称引用

clean:
    rm -f *.o
```

# 5 使用Autoconf工具

- 工具介绍

自己编写`makefile`并使用`make`命令来编译自己写的程序会带来很大的便捷性，然而，一般情况下都是手写一个简单的makefile，但想要写出一个符合自由软件惯例的makefile则没那么容易了。然而，可以使用`autoconf`及`automake`工具来自动生成符合自由软件惯例的makefile，这样就可以像`GNU`程序一样，只要使用`./configure`、`make`、`make install`就可以把程序安装到系统上了。
使用`automake`，开发人员只需要写一些简单的含有预定义宏的文件，由`autoconf`生成`configure`，由`automake`生成另一个宏文件`makefile.in`，再使用`configure`根据`makefile.in`来生成一个符合惯例的`makefile`。

## 5.1 使用介绍

从最简单的`hello world`开始。

- 检查必备工具

需要用到的工具有：
`autoscan`、`aclocal`、`autoconf`、`autoheader`、`automake`。

- 创建目录

为自己的程序创建一个家目录，比如说`hello`。

- 编写程序

新建C语言(当然不限于C语言)文件，编写相应代码，如下：

```c
#include <stdio.h>
int main()
{
    printf("hello world");
    return 0;
}
```

- 执行autoscan生成configure.scan

执行`autoscan`命令，此时将生成`configure.scan`文件。

    autoscan工具用来扫描源代码以搜寻一般的可移植性问题，比如检查编译器、库和头文件等，并创建configure.scan文件。
    它会在给定目录及子目录树中检查源文件，若没有给出目录，就在当前目录及其子目录树中进行检查。
    
- 更名.scan为.ac，并修改相应内容

重命名`configure.scan`文件为`configure.ac`。

- 修改configure.ac文件内容

修改`configure.ac`，更改内容如下：

```ac
#   -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.69])
AC_INIT([FULL-PACKAGE-NAME], [VERSION], [BUG-REPORT-ADDRESS])
AC_CONFIG_SRCDIR([hello.c]) # main函数所在文件
AC_CONFIG_HEADERS([config.h])
AM_INIT_AUTOMAKE(hello, 1.0) # 生成的可执行文件名及版本号

# Checks for programs.
AC_PROG_CC

# Checks for libraries.

# Checks for header files.

# Checks for typedefs, structures, and compiler characteristics.

# Checks for library functions.

AC_OUTPUT(Makefile) # 注意要在此添加生成的makefile文件名，此时为Makefile
```

- 执行aclocal

执行命令`aclocal`，生成`aclocal.m4`及`autom4te.cache/`。

    aclocal工具用于扫描configure.ac文件生成aclocal.m4。
    此工具根据已经安装的宏、用户定义宏和acinclude.m4文件中的宏将configure.ac文件需要的宏，集中定义到文件aclocal.m4中。

- 执行autoconf

执行命令`autoconf`，生成`configure`等文件

    将configure.ac中的宏展开，生成configure脚本。
    这个过程可能用到aclocal.m4中定义的宏。

- 使用autoheader生成config.in.h

执行命令`autoheader`，生成`config.in.h`文件

    autoheader工具负责生成config.h.in文件。该工具会从“acconfig.h”文件中复制用户附加的符号定义。

- 新建Makefile.am

automake工具会根据configure.in.h中的参量把Makefile.am转换成Makefile.in文件。在使用Automake工具前，需要手工创建脚本配置文件Makefile.am，内容如下：

```am
AUTOMAKE_OPTIONS = foreign # automake选项。在执行automake时，它会检查目录下是否存在标准GNU软件包中应具备的各种文件，例如AUTHORS、ChangeLog、NEWS、README等文件。若将其设置成foreign时，automake会改用一般软件包的标准来检查，则不需强制要求生成该文件，若不加这一句，则需手动创建该文件。
bin_PROGRAMS = hello # 指定所要产生的可执行文件的文件名。如果要产生多个可执行文件，那么在各个名字间用空格隔开。
hello_SOURCES = hello.c #  指定产生“hello”时所需要的源代码。如果它用到了多个源文件，使用空格符号将它们隔开。比如需要hello.h，hello.c则写成hello_SOURCES= hello.h hello.c。
# 如果在bin_PROGRAMS定义了多个可执行文件，则对应每个可执行文件都要定义相对的filename_SOURCES
```

- 执行automake

执行命令`automake -a`，生成`Makefile.in`

    automake将会根据configure.in.h和Makefile.am生成Makefile.in。
    -a选项是指add missing standard files to package，他会让automake加入一个标准的软件包来产生必须的文件
    
- 执行configure生成Makefile

执行命令`./configure`，生成`Makefile`。

    执行命令./configure之后，其将会根据Makefile.in生成Makefile文件。
    
- 使用Makefile进行编译

执行命令`make`，生成可执行程序。

在符合GNU Makefiel惯例的Makefile中，包含了一些基本的预先定义的操作：

|命令|说明|
|---|---|
|make|根据Makefile编译源代码，连接，生成目标文件，可执行文件|
|make clean|清除上次的make命令所产生的object文件（后缀为“.o”的文件）及可执行文件。|
|make install|将编译成功的可执行文件安装到系统目录中，一般为/usr/local/bin目录。|
|make dist|产生发布软件包文件（即distribution package）。<br>这个命令将会将可执行文件及相关文件打包成一个tar.gz压缩的文件用来作为发布软件的软件包。<br>它会在当前目录下生成一个名字类似“PACKAGE-VERSION.tar.gz”的文件。<br>PACKAGE和VERSION，是在configure.ac中定义的AM_INIT_AUTOMAKE(PACKAGE, VERSION)。|
|make distcheck|生成发布软件包并对其进行测试检查，以确定发布包的正确性。<br>这个操作将自动把压缩包文件解开，然后执行configure命令，并且执行make，来确认编译不出现错误<br>最后提示你软件包已经准备好，可以发布了。|
|make distclean|类似make clean| clean，但同时也将configure生成的文件全部删除掉，包括Makefile。

## 5.2 过程图示

![](../post_img/58d38053ab64417edb0008a4)

# Reference 

http://blog.csdn.net/baiziyuandyufei/article/details/42917995
http://www.cnblogs.com/itech/archive/2010/11/28/1890220.html