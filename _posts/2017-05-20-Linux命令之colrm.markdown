title: Linux命令之colrm
date: 2017-05-20 22:33:58
updated: 2017-05-21 11:07:28
tags:
- Linux Command
categories:
- Study
- Computer
- OS
- Linux
- Command
---
> `colrm`(column remove)程序从标准输入读取数据，删除指定的数据列，然后将剩余数据写入标准输出。

# 用法

`colrm`的语法为：

```sh
$ colrm [startcol [endcol]]
```

其中，*startcol*和*endcol*指定要移除区域的开头和末尾列编号，列的编号从1开始。

例如，有一个主数据文件，名为**filelist**，这个文件包含了当前目录下的文件长信息。信息如下：

```sh
drwxr-xr-x. 2 maple maple   4096 Apr  3 11:57 Desktop
drwxr-xr-x. 2 maple maple   4096 May 11 19:37 Documents
drwxr-xr-x. 5 maple maple   4096 May 19 13:53 Downloads
drwxr-xr-x. 2 maple maple   4096 Mar  9 19:34 Music
drwxr-xr-x. 5 maple maple   4096 Apr  2 14:04 Pictures
drwxrwxr-x. 2 maple maple   4096 Apr 20 20:37 Podcasts
drwxr-xr-x. 2 maple maple   4096 Mar 12 15:29 Public
drwxr-xr-x. 2 maple maple   4096 Jan  6 21:16 Templates
drwxr-xr-x. 2 maple maple   4096 Jan 12 09:44 Videos
drwxr-xr-x. 2 maple maple   4096 Apr  2 14:08 Wallpapers
drwxr-xr-x. 2 maple maple   4096 Feb  2 22:55 Webcam
```
现在，为了只想获得文件的权限及文件名信息，即只留下这两项信息，则需要移除中间的数据，使用如下命令：

```sh
cat filelist | colrm 11 45
```
输出如下：

```sh
drwxr-xr-x Desktop
drwxr-xr-x Documents
drwxr-xr-x Downloads
drwxr-xr-x Music
drwxr-xr-x Pictures
drwxrwxr-x Podcasts
drwxr-xr-x Public
drwxr-xr-x Templates
drwxr-xr-x Videos
drwxr-xr-x Wallpapers
drwxr-xr-x Webcam
```
也可以不指定*endcol*，那么就会从*startcol*开始移除直到末尾列。
如果不指定起始列也蔑有指定结束列，那么`colrm`不删除任何列。

----------


# Reference

[1] Haeley Hahn. Unix & Linux 大学教程[M]. 张杰良, 译. 北京:清华大学出版社, 2010.1:351.