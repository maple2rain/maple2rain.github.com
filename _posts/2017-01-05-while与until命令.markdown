title: while与until命令
date: 2017-01-05 16:49:24
updated: 2017-01-05 17:02:24
tags:
- Linux Shell
categories:
- Study
- Computer
- OS
- Linux
- API
- Shell
- Structure  Stament
---
## 1 while命令

**while**命令某种意义上是**if-then**语句与**for**循环的混杂体。
**while**命令允许定义一个要测试的**test**命令，只要定义的**test**命令返回的退出状态码是0，则会迭代的开始测试命令，否则停止。

### 1.1 格式

```sh
while test command
do
    commands
done
```

- 关键

    如果**test**命令的退出状态码必须随着循环中运行的命令改变。
    如果从来不变，那么将陷入死循环。

### 1.2 简单例子

```sh
var = 1

while [ $var -lt 10 ]
do
    echo $var
    #变量自增1
    var=$[ $var + 1 ]
done
```

上述例子中，定义了**test**命令**[ $var -lt 10 ]**。
此时**while**命令会判断**var**是否小于10；如果是则将其输出。
![](../post_img/586e0892ab6441209e0047a7)

### 1.3 使用多个test命令

**while**命令允许定义多个**test**命令语句。
每次迭代中所有的测试命令都会被执行，直到最后一个测试命令的退出状态码会被用来决定什么时候结束循环。

## 2 until命令

**until**命令与**while**命令工作方式完全相反。
**until**命令要求指定一个通常**输出非零退出状态码**的**test**命令。
只有**test**命令的退出状态码非零，**shell**才会执行循环中的命令，直到**test**命令的退出状态码为0。

### 2.1 格式

```sh
until test command
do
    commands
done
```

### 2.2 使用多个test命令

与**while**命令功能相同，可以指定多个**test**命令，只有最后一个命令的退出状态码决定**shell**是否执行循环中的命令。