---
layout: post
author: LPF
title: Linux命令之shred
date: 2017-05-23 21:30:49
updated: 2017-05-23 22:08:02
tags:
- Linux Command
categories:
- Study
- Computer
- OS
- Linux
- Command
---
> 删除文件，还是有机会恢复文件。如果想真正删除**文件**，恐怕只有毁掉设备了吧。不，还可以用`shred`。

# 前言

一旦删除文件，就没有办法再找回这个文件了。因为`rm`不会将文件放进回收站，而是直接将其**抹去**。但是，所谓的抹去，并没有将实际的磁盘空间内容清除，文件系统只是将这部份磁盘空间标识为可以重用。最终，这部份磁盘空间将被重用，旧数据将被新数据覆盖。
一般情况下，对于忙碌的大型Unix系统，这可能只需要几秒钟的时间。但是，无法确定这种情况何时会发生，有时候旧数据可能会在未使用部分隐藏很长一段时间。实际上，有一些特殊的**恢复删除**工具能够查看磁盘未使用的部分，并恢复旧数据。
此外，即使数据被覆盖了，在极端的情况下数据也有可能恢复，只要数据没有被多次覆盖。如果将硬盘拿到拥有非常昂贵的数据恢复设备的实验室中，则有可能通过分析硬盘磁面的磁迹恢复硬盘上被覆盖过的旧数据。
然而，`shred`程序正是来拯救您的硬盘的。

----------


# 用法

`shred`的语法为：

```sh
$ shred -fvuz [file...]
```
其中，*file*是文件的名称。

`shred`程序的目的就是多次覆盖硬盘上已有的数据，从而使世界上最昂贵的数据恢复设备也难以记录硬盘磁面的磁迹。但记住此时他并未删除该文件。文件还存在，只不过被无意义数据替代。

----------


# 选项

|选项|含义|解释|
|---|
|-f|force|忽略受限制的文件权限，强制执行|
|-u|ultimate|在处理后最终删除文件|
|-v|verbose|处理过程中显示处理信息|
|-z|zero|在结束处理时将文件全部填充为0|
 
 

----------

# Reference

[1] Haeley Hahn. Unix & Linux 大学教程[M]. 张杰良, 译. 北京:清华大学出版社, 2010.1:669-670.
