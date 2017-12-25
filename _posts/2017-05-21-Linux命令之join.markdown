title: Linux命令之join
date: 2017-05-21 22:11:20
updated: 2017-05-21 23:14:16
tags:
- Linux Command
categories:
- Study
- Computer
- OS
- Linux
- Command
---
> 在所唷针对有序数据专门设计的Unix过滤器中，最有趣的过滤器就是**join**了。他基于特定字段将两个有序文件组合在一起，让人想起数据库的**join**。

# 用法

`join`程序的语法为：

```sh
$ join [-i] [-a1|v1] [-a2|v2] [-1 field1] [-2 field2] file1 file2
```

其中，*field1*和*field2*是引用特定字段的数字，*file1*和*file2*是包含**有序数据**的文件的名称。

----------


# 举例

在详细介绍之前，先示范一个例子。假设您有两个有序文件，这两个文件都有许多人的信息，其中每个人都有一个唯一的标识号。
在第一个文件*names*中，每行包含一个*ID*号以及这个人的姓氏和名字：

```sh
1 Hugh Mungus
2 Stew Pendous
3 Mick Stup
4 Melon Collie
```

在第二个文件*phone*中，每行包含一个*ID*号以及一个电话号码：

```sh
1 101-555-1111
2 202-555-2222
3 303-555-3333
4 404-555-4444
```

`join`程序允许基于两个文件中共同的值组合文件，在这个例子中，这个共同的值就是*ID*号：

```sh
$ join name phone
1 Hugh Mungus 101-555-1111
2 Stew Pendous 202-555-2222
3 Mick Stup 303-555-3333
4 Melon Collie 404-555-4444
```

当`join`的读取输入时，他忽略前导空白符，也就是行头的空格和制表符。
当基于匹配的字段组合两组数据时，我们称之为**联接**(来自数据库理论)。用来匹配的具体字段称为**联接字段**(join field)。默认情况下，`join`假定联接字段是每个文件的第一个字段。此外，`join`假定每行的各个字段之间用空白符分隔，也就是一个或多个空格或者制表符。
为了创建一个联接，程序需要在两个文件中查找行对，也就是说联接字段拥有相同值的行。对于每个行对来说，`join`生成一个包含3部分的输出：

1. 公共联接字段值
2. 第一个文件中行的剩余部分
3. 第二个文件中行的剩余部分

在上面的例子中，第一个文件中的每一行都与第二个文价中的某一行匹配，但现实可能并不总是这种情况。例如，考虑下述文价，您在生成一个朋友们的生日即喜爱礼物的列表。
第一个文件*birthday*中包含两个字段，名字和生日：

```sh
Al      5-10-1994
Barbara 2-2-1994
Dave    3-8-1992
Frances 10-12-1995
George  1-18-1992
```

第二个文件*gifts*也包含两个字段，名字和礼物：

```sh
Al      money
Barbara chocolate
Charles music
Dave    book
Edward  camera
```

在这个例子中，*Al*、*Barbara*和*Dave*的生日信息和礼物信息都有了，但是*France*和*George*没有礼物信息，而*Charles*和*Edward*没有生日信息，对他们使用`join`程序，结果如何呢？

```sh
$ join birthday gifts 
Al 5-10-1994 money
Barbara 2-2-1994 chocolate
Dave 3-8-1992 book
```

----------


# 外联接

因为只有3行匹配的联接字段，所以输出只有三行。
但是，假定您希望查看所有人的生日信息，即便他们没有礼物信息，则可以使用`-a`(all)选项，后面跟一个**1**，这将告诉`join`输出第一个文件中的所有名字，即使他们没有礼物信息：

```sh
$ join -a1 birthday gifts
Al 5-10-1994 money
Barbara 2-2-1994 chocolate
Dave 3-8-1992 book
Frances 10-12-1995
George 1-18-1992
```

同样，如果希望了解所有人的礼物信息，即使他们没有生日信息，则可以使用`-a2`：

```sh
$ join -a2 birthday gifts 
Al 5-10-1994 money
Barbara 2-2-1994 chocolate
Charles music
Dave 3-8-1992 book
Edward camera
```

为了显示两个文件中的所有名字，可以同时使用两个选项：

```sh
$ join -a1 -a2 birthday gifts
Al 5-10-1994 money
Barbara 2-2-1994 chocolate
Charles music
Dave 3-8-1992 book
Edward camera
Frances 10-12-1995
George 1-18-1992
```

当以默认方式使用`join`时，所获得的结果称为**内联接**，而使用`-a1`或`-a2`选项时，输出称为**外联接**。这是很有趣的数据库理论，有兴趣可以学习一下[数据库](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%BA%93)、[SQL](https://en.wikipedia.org/wiki/SQL)。

----------


# 选出不匹配

如果只希望查看那些不匹配的行，可以使用`-v1`或者`-v2`(reverse)选项。当使用`-v1`选项时，`join`程序只输出第一个文件中不匹配的行，而忽略匹配的行，例如：

```sh
$ join -v1 birthday gifts
Frances 10-12-1995
George 1-18-1992
```

当使用`-v2`选项时，结果是第二个文件中不匹配的行：

```sh
$ join -v2 birthday gifts
Charles music
Edward camera
```

当然，也可以同时使用两个选项，获得两个文件中不匹配的行：

```sh
$ join -v1 -v2 birthday gifts
Charles music
Edward camera
Frances 10-12-1995
George 1-18-1992
```

----------


# 有序

因为`join`依赖于数据是否有序，所以还可以指定`-i`选项忽略大小写等。一般，使用`sort`程序为`join`提供数据。

----------


# 指定联接字段

除了默认联接字段是每个文件的第一个字段，还可以通过`-1`和`-2`选项指定使用不同的联接字段。
为了改变第一个文件的联接字段，可以使用`-1`选项，后面跟希望使用的字段编号。
例如，下述命令联接两个文件*data*和*statistics*，所使用的联接字段是第一个文件的第3个字段和第二个文件的第1个字段(默认)：

```sh
$ join -1 3 data statistics
```

为了改变第二个文件的联接字段，可以使用`-2`选项。
例如，下述命令使用第一个文件的第3个字段和第二个文件的第4个字段进行联接：

```sh
$ join -1 3 -2 4 data statistics
```

> 再次提醒
**join**期望的是有序数据，因此所获结果依赖于区域设置和排序序列。

----------


# Reference

[1] Haeley Hahn. Unix & Linux 大学教程[M]. 张杰良, 译. 北京:清华大学出版社, 2010.1:425-429.