---
layout: post
author: LPF
title: Linux命令之locate
date: 2017-05-23 22:57:41
updated: 2017-05-23 23:09:17
tags:
- Linux Command
categories:
- Study
- Computer
- OS
- Linux
- Command
---
> 查找文件最快捷的方式，莫过于使用基于搜索数据库的方式了。`locate`正是这样一个较为**强大**的程序。

# 用法

`locate`程序的任务就是搜索一个特殊的数据库(该数据库中包含所有**可公共访问**的文件的路径名)，查找所有包含特定模式的路径名。该数据库自动维护，并定期更新。
`locate`程序的语法为：

```sh
$ locate [options] pattern...
```
其中，*pattern*是在路径名中查找的模式。

假设您要搜索路径名中包含**test**的文件，可以采用如下命令，但由于这类文件非常多，因此最好将命令的输出管道传送给`less`：

```sh
$ locate test | less
```

----------

# 选项

|选项|含义|解释|
|---|
|-b|basename|基名或文件名，匹配路径名最后部分符合查找模式|
|-c|count|显示匹配文件的总数，而不显示实际的文件名|
|-i|ignorecase|忽略大小写|
|-r|regular expression或regex|使用正则表达式|
|-S|statistics|显示统计数据库信息，不接*pattern*|
 
 

----------

# 不足

尽管`locate`简单易用，但其有个缺点，就是该数据库是定期更新的，新创建的文件并不会出现在数据库中，所以也就不会被搜索到。直至下一次数据库更新时才可以做到。因此，可以手动进行数据库更新，但需要超级用户权限：

```sh
$ sudo updatedb
```

为了避免这一限制，还可以使用`find`程序，因为`find`实际上搜索整个目录树。

----------

# Reference

[1] Haeley Hahn. Unix & Linux 大学教程[M]. 张杰良, 译. 北京:清华大学出版社, 2010.1:678-679.