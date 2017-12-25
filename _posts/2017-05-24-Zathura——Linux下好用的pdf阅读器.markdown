title: Zathura——Linux下好用的pdf阅读器
date: 2017-05-24 22:54:49
updated: 2017-05-25 22:07:10
tags:
- Tool
categories:
- Study
- Computer
- Tools
---
> 要知道，Linux下神器无所不有，但对于这款pdf阅读器，我还真是有点爱不释手。除了名字有点拗口之外🙁

# 键盘操作

最让我想要推荐的原因，莫过于他可以使用全键盘操作。使用与`vim`类似的命令绑定键，可以在无需鼠标的情况下就能完成你想要的操作。(当然选中复制除外，不知道是否真有这个命令为没发现)。

# 安装

## 手动安装

参考[官网](https://pwmt.org/projects/zathura/installation/)。
进入[下载页面](https://pwmt.org/projects/zathura/download/)，选择你想要的版本(一般是最新)。
执行安装命令：

```sh
$ tar xfv zathura-<version>.tar.gz
$ cd zathura-<version>
$ make
$ make install
```

## 快速安装

对于Fedora，直接使用`dnf`进行下载即可：

```sh
$ sudo dnf install zathura
```

可以进入[Git](https://git.pwmt.org/groups/pwmt)查看小组开发情况。也可查看其[源码](https://github.com/pwmt/zathura)。

# 快捷键

## 视图变换

|按键|解释|
|---|
|a|放大页面到合适大小|
|s|放大页面到窗口宽度|
|d|切换到双页视图|
|F5|改变当前页面状态|
|F11|切换为全屏模式|
|R|重新加载文档|
|r|顺时针旋转90度|
|^r|反转颜色|
|+或zI|放大(zoom in)|
|-或zO|缩小(zoom out)|
|=或z0|恢复初态(zoom to original)|

## 移动

注释：页指页数的一页，面指屏幕显示的一面

|按键|解释|
|---|
|J|切换到下一页|
|K|切换到上一页|
|h或←|向左滚动|
|j或↓|向下滚动|
|k或↑|向上滚动|
|l或→|向右滚动|
|gg|跳转到第1页|
|nG|跳转到第n页，n为数字|
|GG|跳转到最后一页|
|H|跳转到当前页的顶部|
|L|跳转到当前页的底部|
|^t|向左滚动半面|
|^d|向下滚动半面|
|^u|向上滚动半面|
|^y|向右滚动半面|
|t|向左滚动一面|
|^f或space|向下滚动一面|
|^b|向上滚动一面|
|y|向右滚动一面|

## 功能

|按键|注释|
|---|
|/或?|搜索|
|n或N|向后或向前匹配搜索|
|o或O|打开文档|
|f|跟踪链接|
|F|显示链接目标|
|:|键入命令|
|Tab|显示索引或进入索引模式，在索引中进行移动|
|^m|切换输入栏|
|^n|切换状态栏|
|mX|设置标记为字母或数字X|
|'X|到达标记X处(单引号)|
|q|退出|



# Reference

[1] [Zathura官网](https://pwmt.org/projects/zathura/)
[2] [用Zathura来看PDF](http://archive.3zso.com/archives/zathura-pdf-reader.html)
[3] [超级好用的pdf阅读器推荐：Zathura](https://www.douban.com/group/topic/22778587/)