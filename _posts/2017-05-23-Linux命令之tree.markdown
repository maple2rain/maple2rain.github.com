title: Linux命令之tree
date: 2017-05-23 17:14:32
updated: 2017-05-23 17:26:36
categories:
- Study
- Computer
- OS
- Linux
- Command
---
> `tree`是一个功能强大的工具，其可以绘制文件系统任何部分的图形。

# 用法

`tree`的语法为：

```sh
$ tree [-adfFilrst] [-L level] [directory...]
```
其中，*level*是树的深度，而*directory*是目录的名称。

为了查看该程序的工作方式，可以列举整个文件系统的树。因为该树特别大，所以需要将其输出传送给`less`(展示部分)：

```sh
$ tree / | less
/
├── 1
├── bin -> usr/bin
├── boot
│   ├── config-4.10.13-200.fc25.x86_64
│   ├── config-4.10.14-200.fc25.x86_64
│   ├── config-4.10.15-200.fc25.x86_64
│   ├── efi [error opening dir]
│   ├── elf-memtest86+-5.01
│   ├── extlinux
...
```

----------

# 选项

|选项|含义|解释|
|---|
|-a|all|显示所有文件，包括隐藏文件|
|-d|directory|只显示目录|
|-f|full|显示完整的路径名|
|-F|flag|显示文件类型标识|
|-i|ignore|省略缩进|
|-l|link|显示符号连接|
|-L level|limit|限制树的深度为level|
|-r|reverse|反序输出|
|-s|size|显示文件名的同时显示大小|
|-t|time|按修改时间顺序排序输出|

# 查找某个目录

假如，为了查找文件系统中所有命令为**bin**的目录，可以使用`tree`。这里的思想就是从根目录开始，限制只搜索目录(`-d`)，显示完整的路径名(`-f`)，省略缩进(`-i`)，然后将输出发送给`grep`，并只选择那些以**/bin**结尾的行。命令为：

```sh
$ tree -dif / | grep '/bin$'
```

----------

# Reference

[1] Haeley Hahn. Unix & Linux 大学教程[M]. 张杰良, 译. 北京:清华大学出版社, 2010.1:641-643.
