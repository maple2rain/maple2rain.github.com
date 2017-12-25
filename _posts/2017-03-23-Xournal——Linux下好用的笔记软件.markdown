title: Xournal——Linux下好用的笔记软件
date: 2017-03-23 21:51:51
updated: 2017-04-28 11:03:48
tags:
- Tool
categories:
- Study
- Computer
- Tools
---
# 简介

由于困扰于`Foxit Reader`打开缓慢的问题，所以一直在寻找替代阅读器。实际上默认安装的`Document Viewer`已经足够满足日常的阅读需求了，然而有时还是需要对文档进行注释，以便后期再次阅读时方便唤醒记忆。
无意间，在网上搜索时发现了`Xournal`，没想到他还挺不错。

# 功能介绍

![主页面](../post_img/58d3d826ab64417edb0010ca)

- 主要功能
    - 记文本笔记
    - 铅笔涂鸦等
    - 绘制线条
- 工具
    - 钢笔
    - 荧光笔
    - 橡皮擦
    - 尺子
    - 图形识别工具
    - 选择工具
- 额外功能
    - 鼠标按键绑定操作
        
        在`Options`中，可分别对`Button * Mapping`进行操作绑定，其中`*`为数字，2表示中键，3表示右键
    - 支持手写板
    
# PDF注释

在工具栏中选择`File->Annotate PDF`，即可打开pdf并进行注释，在进行相应的操作后，可在工具栏中选择`File->Export to PDF`或按快捷键`Ctrl-E`进行PDF导出。

# 下载安装

在`Fedora`中，已经有包被包含进仓库了，所以下载安装十分方便，直接安装即可：

```sh
$ sudo dnf install xournal
```

也可以直接从[官网](http://xournal.sourceforge.net/)下载安装。