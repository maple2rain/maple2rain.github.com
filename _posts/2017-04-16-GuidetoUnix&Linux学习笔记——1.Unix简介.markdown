---
layout: post
author: LPF
title: Guide to Unix & Linux学习笔记—— 1.Unix 简介
date: 2017-04-16 10:40:52
updated: 2017-05-19 16:17:53
categories:
- Study
- Computer
- OS
- Linux
- Guide to Unix & Linux 
---
# 使用Unix的原因

- 使用Unix，可以决定如何使用计算机以及希望在细节深入到何种程度

    用户可以按照自己的方式使用计算机，不用再按照他人(微软公司、IBM公司或者父母)设置的方式使用计算机。可以根据系统是否适合自己来定制系统，而且还可以选择许多设计出色的工具和应用程序。
    
- 使用Unix将改变思考方式，并向好的方向转变
- 作为全球Unix社区的一名成员，将有可能学习到如何使用一些目前人类发明的最好工具
- 可以连续数月使用一台计算机而不必重启，不必担心计算机系统的崩溃、失去响应或者意外停止
- 除非管理大型的网络，不必考虑以下令人恼火的问题
    - 计算机病毒
    - 间谍软件
    - 运行失控的程序
    - 为了保持计算机平稳运行而必须执行的神秘、无法理解的规定程序
    
- 如果是一名程序员，可以有一大批基于Unix的神奇工具可以用来帮助开发、测试及运行程序
    - 拥有与语言相关的插件的文本编辑器
    - 脚本解释器
    - 编译器(包括交叉编译器)
    - 调试器
    - 仿真器
    - 语法分析程序生成器
    - GUI构建器
    - 如见配置管理器
    - 错误跟踪软件
    - 编译管理器
    - 文档工具
    - 相应的社区、讨论组、综合软件文档

- 所有的软件都是免费的

----------


# Unix语言

Unix系统第一语言是英语。但Unix系统和文档已经被翻译为许多其他语言，因此只要系统以自己的语言运行，就不必精通英语。但如果进入世界范围的基于Unix社区，就会发现大多数信息和许多讨论组都使用英语。
另外，Unix社区还创造了他自己的许多新单词。

----------


> 名称含义
**Unix**
20世纪60年代，贝尔实验室（属于AT&T公司）的一些研究人员在麻省理工学院开发一个称为Multics的项目，即一种早期的分时操作系统。Multics是一个协作项目，程序员包括麻省理工学院，通用电气公司和贝尔实验室的人员。
Multics是“Multiplexed Information and Computing Service”的首字母缩写(Multiplex指将多个电子信号组合成一个单独的信号)。
到20世纪60年代末，贝尔实验室的管理部门决定不再继续支持Multics项目，并将他们的研究人员撤回贝尔实验室。1969年，这些研究人员中的一名研究员Ken Thompson为微型计算机PDP-7开发了一个简单的小型操作系统，在为该操作系统寻找名称时，Thompson将他的系统和Multics进行了对比。
Multics的目标是在同一时间为多个用户提供众多功能。Multics非常庞大，难以使用，而且还有许多问题。
Thompson的系统比较小、要求较低（至少在刚开始时），而且一次只能由一人使用。另外，系统的每个部件只限于完成一件事情，并且出色地完成这件事情。Thompson决定将他的系统命名为Unics(“Uni”意味着“一个”)。后来很快又将Unics修改成Unix。
换句话说，Unix这个名称是Multics的双关语。

----------


# 不知道正在使用Unix的人

大多数使用Unix的人不知道他们正在使用Unix系统，因为Unix可以再许多不同类型的计算机系统上使用，而且这些系统运行得非常出色。
例如，大多数Web服务器运行在某种类型的Unix上。当访问网站时，人们通常意识不到使用的就是Unix，至少无法直接意识到。另外，Unix可以用来运行所有类型的机器，不仅包括所有规模的计算机(从最大的大型机到最小的手持式设备)，而且还有嵌入式或实时系统，例如仪表、收银机等。
最后，大多数支持Internet的机器也运行Unix，如路由器、邮件服务器、Web服务器等。
最有趣的是，Mactintosh OS X系统正是基于一种称为FreeBSD的Unix。

----------


# 学习提示

要想学习Unix的所有方面是不可能的，在学习Unix时要集中在需要的以及感兴趣的地方。

- 希望定制自己的工作环境

    - 最好阅读Unix工作环境(第6章)
    - 理解所谓的shell(第11章)
    - 使用shell的一些细节内容(第12、13、14章)

    然后，就可以通过修改特定的文件来定制自己的工作环境了。

- 为了修改文件，还需要学习其他内容

    - 理解如何启动一个工作会话(第4、5章)
    - 理解如何在Unix使用键盘(第7章)
    - 知道如何使用文本编辑程序(第22章)
    - 理解文件系统(第23章)
    - 显示文件的命令(第21章)
    - 管理文件的命令(第24、25章)
    
很明显，上述的学习顺序并不会快速地引人入门，但是他确实强调了在刚开始时就需要理解的最重要的原则：`设计Unix的目的不是为了学习而是为了使用`。换句话说，学习Unix费时费力。但是，一旦掌握了需要掌握的技能，无论用Unix从事什么样的工作都会既快又方便。
重要的是，首先学习基本知识，然后再学习希望学习的内容，顺序可以由自己决定。

----------


# Reference 

[1] Haeley Hahn. Unix & Linux 大学教程[M]. 张杰良, 译. 北京:清华大学出版社, 2010.1:1-7.