---
layout: post
author: LPF
title: codeblocks常见问题
date: 2017-01-14 11:05:11
updated: 2017-05-09 17:14:32
tags:
- ComProblem
categories:
- Study
- Computer
- Tools
---
## 1 缩进异常

- 关闭IDE
- 安装插件`codeblocks-contrib`
    
    在Fedora下，安装方式为`sudo dnf install codeblocks-contrib`    

- 重启IDE

## 2 修改xTerm

在运行程序时，codeblocks默认的运行终端是`xTerm`，若想修改为其他终端，可在
<font size = 2><B>Settings -> Environment settings -> General settings -> Terminal to launch console programs</B> </font>
进行选择相应的终端。

## 3 修改主题

在Fedora中，主题文件`default.conf`存放于`～/.config/codeblocks/`。可在[官网](http://wiki.codeblocks.org/index.php?title=Syntax_highlighting_custom_colour_themes)复制主题文件并保存为`default.conf`，或者在我的[百度云](https://pan.baidu.com/s/1ge8mnD5)直接下载，且放置于主题文件目录下。
最好备份一下原来的文件，以在操作错误时进行恢复操作。
