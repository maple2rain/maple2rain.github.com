title: Guide to Unix & Linux学习笔记—— 2.什么是Linux？什么是Unix？
date: 2017-04-16 12:01:16
updated: 2017-05-19 16:17:50
categories:
- Study
- Computer
- OS
- Linux
- Guide to Unix & Linux 
---
# 什么是Linux？什么是Unix？

最简短的答案就是：`Unix是一种类型的计算机系统，而Linux是Unix系统的一个特定家族的名称`。

----------


# 什么是操作系统

计算机按照指令自动执行任务。一系列指令称为`程序`。因为计算机遵循指令，所以我们称计算机在`运行`或者`执行`程序。一般而言，程序被称为`软件`，而计算机的物理部件被称为`硬件`。计算机硬件包括系统主板、磁盘驱动器、键盘、鼠标、显示器、屏幕、打印机等。
`操作系统`（属于软件）是运行计算机的总控制程序。操作系统的主要功能是高效地利用硬件。为了完成这一任务，操作系统充当硬件的基本接口，既为使用计算机的用户提供界
面，也为正在执行的程序提供界面。
无论何时，当计算机启动并运行时，操作系统就存在，等待提供服务，并管理计算机的资源。

更精确地说，操作系统最重要的功能包括：

- 控制计算机并在计算机启动或者重新启动时初始化计算机;初始化过程只是引导过程的一部分
- 支持与计算机交互所使用的界面（文本或者图形）
- 为需要使用计算机资源（磁盘空间、文件位置、处理时间、内存等）的程序提供接口
- 管理计算机的内存
- 维护并管理文件系统
- 调度工作
- 提供账户和安全服务

作为一个家族，所有Unix操作系统都有两个重要特征：多任务和多用户。`多任务`意味着Unix系统可以同时运行不止一个程序。`多用户`意味着Unix可以同时支持不止一个用户(Windows系统是一个多任务、单用户的操作系统)。

----------


> 名称含义
**引导**
术语“引导(booting)”是bootstrapping的简写,表示一个古老的谚语“通过自力更生出人头地”。例如,“在Bartholomew失去了所有的金钱之后,他好长一段时问非常贫困,但是,通过勤奋工作,经过自力更生他又出人头地了,成为了一个成功的细燕麦粉蛋糕商人。”
引导的思想就是一个困难的,复杂的目标可以通过一个小的动作开始,然后以这个小的动作为基础。一步一步地到达期望目标而完成。
计算机系统就是以这种方式启动的。当打开计算机的电源(或者重新启动计算机)时,一个单独的、小型的程序自动运行,这个程序启动另一个程序,一个更复杂的程序,然后逐步递进。最终,操作系统(一个非常复杂的程序)接过控
制,完成初始化过程。

----------


# 内核

当计算机启动时，计算机要经历一系列的动作，这些动作构成[引导过程](http://maple2rain.leanote.com/post/Linux%E5%90%AF%E5%8A%A8%E8%BF%87%E7%A8%8B)。该过程最后一个动作时启动一个非常复杂的程序，这个程序被称为`内核`(kernel)。
内核的作用是控制计算机，充当操作系统的核心。由于这一点，内核总是一直运行。除非关闭了计算机系统，否则内核会一直运行，通过这种方式，内核一直可用，并在需要时提供基本的服务。
内核是操作系统的核心，本质可能根据操作系统的不同而有所区别，但是内核所提供的基本服务再各个操作系统之间都基本相同，这些服务包括：

- 内存管理(虚拟内存管理,包括分页)
- 进程管理(进程创建、终止、调度)
- 进程间通信(本地、网络)
- 输入/输出(通过设备驱动程序,即实现与物理设备实际通信的程序)
- 文件管理
- 安全和访问控制
- 网络访问(如TCP/IP)

## 单内核

单内核由一个非常庞大的程序构成,该程序自身可以完成所有的事情。微内核是一个非常小的程序,只能执行最基本的任务。
单内核的优点是它的速度比较快:所有的事情都在一个单独的程序中完成,这样将比较高效。但是单内核的缺点是规模较大而且使用不便,从而使这类内核难以设计和维护。

## 微内核

为了执行其他功能,微内核要调用其他程序,这些程序称为服务器(server)。
微内核比较慢,这是因为它必须调用服务器来完成它的大部分工作,这样效率就不高。但是,因为采用了模块化设计,所以微内核易于程序员的理解,而且针对新系统修改微内核也比较快。微内核还有一个优点,即相比于单内核,它们更易于定制。

大多数Unix系统使用某种类型的单内核，但是一些Unix诸如OS X使用的是微内核。

----------


# Unix=内核+实用工具

对于Unix来说，**内核之外的其他内容**包含大量的辅助程序，这些程序包含在程序包中，可以分成若干种不同的类别。
其中最重要的程序是那些为用户提供使用计算机的界面的程序。这些程序是shell和GUI。

- shell是一种提供基于文本的界面的程序:您可以一个接一个地键入命令,shell读取命令,然后完成所需的工作来执行命令。
- GUl(graphical user interface,图形用户界面)是一个更复杂精美的程序,使用窗口、鼠标指针、图标等提供图形界面。

其他程序称为`Unix实用工具`，每个实用工具(也就是每个程序)都是一个单独的工具。Unix实用工具在每个Unix系统上的工作都相当一致，这意味着，在很大程度上，如果知道使用一种类型的Unix，便知道所有实用工具的使用。
那么，什么是Unix？

----------


> Unix是一种类型的操作系统,它使用Unix内核,并且提供有许多Unix实用工具以及一个Unix shell。实际上,大多数(但并不是全部)Unix都提供有shell以及至少一个GUI。非正式地讲时,我们通常使用术语“实用工具”来包含shell,因此,可以将Unix定义为:
Unix = Unix内核 + Unix实用工具

----------


# 自由软件基金会

1985年，一个名叫Richard Stallman的程序员专家及有思想的社会批评家，组织了一小批程序员，启动了一个称为`自由软件基金会`(Free Software Foundation, FSF)的组织。Stallman的指导原则是**计算机用户应该能够自由地修改软件以适应自己的需求，并且自由共享软件，因为帮助他人是基本的社会责任**。
重要的是要理解**Free Software**的含义，他所指的不是免费，而是自由。任何人都可以对自由软件进行检查、修改、共享以及发行。但是，如果有些人对他们的服务收费，或者要其他人对软件发行付费，也没有什么不对。按Stallman解释，即**谈话免费，但啤酒不免费**。
因为单词**Free**既指自由又指免费，所以多少会有点模糊。因此，为了避免任何潜在的混淆，自由软件现在又称为`开放源代码软件`(Open Source Software)。
Stallman希望FSF的产品能够平稳地融入到目前流行的编程文化中,因此他决定新的操作系统应该与Unix兼容,也就是说它看上去就像Unix系统一样,并且还能够运行Unix程序。按照他曾经工作过的编程社区的传统,Stallman为他还没有构建的操作系统选择了一种古怪的名称。他将这一操作系统称为GNU。

----------


> 名称含义
**GNU**
GNU是Stallman选择用来描述自由软件基金会所开发的一个完全类Unix操作系统的项目的名称,名称本身是**GNU’s Not Unix**只取首字母的缩写词,发音为*ga-new*(它与打喷嚏时的声音很押韵)。
注意,在表达式**GNU's Not Unix**中,单词GNU可以无限地扩展下去:
GNU
(GNU's Not Unix)
((GNU's Not Unix) Not Unix)
(((GNU's Not Unix) Not Unix) Not Unix)
((((GNU's Not Unix) Not Unix) Not Unix) Not Unix)
因此,GNU实际上是一个递归的缩写词(递归指根据其自身来进行定义的内容)。

----------


# GNU宣言摘录

在FSF成立不久，Stallman撰写了一篇小论文。在这篇论文中，他解释了促进自由软件思想的原因。他称之为`GNU宣言`。
他的基本思想——所有的软件应该自由共享——听起来非常天真,但是,随着Internet的兴起,开放源代码软件(即Stallman所谓的“自由软件”)的开发和发行已经成为我们这个世界一个重要的经济和社会力量。差不多有数万个程序可自由使用,它们对世界的贡献非常巨大,难以用金钱衡量,而且编写这些程序的程序员也对此非常兴奋。

----------


> GNU宣言摘录
“我认为:如果我喜欢一个程序的话,那我就应该将它分享给其他喜欢这个程序的人。这句话是我的座右铭。软件商想各个击破用户,使他们同意不把软件和他人分享。我拒绝以这种方式破坏用户之间的团结。我的良心使我不会签下一个不开放的合约或是软件许可证协议。在麻省理工学院人工智能实验室工作的多年时间里,我一直反对这样的趋势与冷漠,但是最后事情糟糕到:我无法在一个处理事情的方法与我的意愿相违背的机构呆下去。”
“为了能继续使用电脑而不感到羞愧,我决定将一大堆自由软件集合在一起,从而使我可以不必再使用不自由的软件。因此,我辞去了人工智能实验室的工作,不给麻省理工学院任何法律上的借口来阻止我把GNU送给其他人......”
“很多程序员对系统软件的商业化感到不悦。这虽然可以使他们赚更多的钱,但是这使他们觉得自己与其他程序员处于对立状态,而不是同志之间的感觉,程序员对友谊的最基本表现就是共享程序,而当前的市场运作基本上禁止程序员将其他程序员作为朋友看待。软件购买者必须在友谊和守法之间做一选择。自然地,有很多人选择了友谊,但是那些相信法律的人常常没办法安心地做这一选择。他们会变得愤世嫉俗,认为写程序只不过是赚 钱的一种方法而已。”
“复制全部或者部分程序对程序员来说就和呼吸一样是自然有益的事。复制软件就应该这么自由......”
“从长远来看,免费提供软件是迈向大同世界的一步,在那个时代中,没有人再为了生计而努力工作。在每周10小时的主要劳动(如立法,家政服务,机器人修理和行星观察)之后,人们自由地参与各种自己感兴趣的活动,例如编程。那时候就不必靠写程序来过活了......”[全文](http://www.cnblogs.com/dyllove98/archive/2005/09/28/2462125.html)

----------


# Linux出现

1991年,Linus Torvalds是Helsinki大学计算机科学系的二年级学生。与其他数以万计的喜欢摆弄编程的学生一样,Linus阅读了Andrew Tanenbaum的书《Operat-ing Systems: Design and Implementation》,该书解释了Minix的设计原则。作为该书的一个附录,Tanenbaum在书中包含了其操作系统的12 000行源代码,Linus花费了许多时间来研究这些代码。
像其他许多程序员一样.Linus希望一个自由(开放源代码)版本的Unix。但是Linus又不像其他程序员,他不愿意再等待了。
1991年8月25日,Linus向Usenet讨论组(也是Minix的论坛,comp.os.minix)发了一个消息。回顾过去,这个短消息已成为Unix世界的历史文档之一。要知道英语是Linus的第二语言,所以这篇文章写得并不正规。
很清楚,Linus只是试图通过构建自己的操作系统来寻找乐趣。他意识到自己的主要工作是编写内核,因为在很大程度上,自由软件基金会已经提供了各种实用工具,在编好内核之后,他可以直接使用这些实用工具。

----------


> LINUS TORVALDS ANNOUNCING HIS NEW PROJECT
From: torvalds@klaava.Helsinki.FI (Linus Benedict Torvalds)
Newsgroups: comp.os.minix
Subject: What would you like to see most in minix?
Summary: small poll for my new operating system
Date: 25 Aug 91 20:57:08 GT
Organization: University of Helsinki
Hello everybody out there using minix -
I'm doing a (free) operating system (just a hobby, won't be big and
professional like gnu) for 386(486) AT clones. This has been brewing
since april, and is starting to get ready. I'd like any feedback on things
people like/dislike in minix, as my OS resembles it somewhat (same
physical layout of the file-system (due to practical reasons) among other
things).
I've currently ported bash(1.08) and gcc(1.40), and things seem to work.
This implies that I'll get something practical within a few months, and I'd
like to know what features most people would want. Any suggestions
are welcome, but I won't promise I'll implement them :-)
Linus (torvalds [at] kruuna.helsinki.fi)
PS. Yes – it's free of any minix code, and it has a multi-threaded fs. It
is NOT portable (uses 386 task switching etc), and it probably never will
support anything other than AT-harddisks, as that's all I have :-(.

----------


1991年9月,Linus发行了第一版的内核,Linus将这个内核称为Linux。Linux通过Internet发行,而且在极短的时间内,Linus开始一个接一个地发行新版本的Linux。全世界的程序员们开始加入到Linus的行列,首先数十人,然后数百人,最终达到了数万人。
这里有一件有趣的事情需要提及,那就是Linus选择使用单内核来设计Linux。由Andrew Tanenbaum设计的Minix使用的是微内核。
最终,Linux变得非常流行,运行在每一种类型的计算机上:不仅运行在PC上,而且还运行在安装有小型内置处理器的手持设备以及大规模并行超级计算机集群(世界上最快、功能最强大的系统)上。实际上,到2005年,几乎世界上每个计算领域都存在着运行某种类型的Linux的机器,Linux成为世界上最流行的操作系统(Windows的应用更加广泛,但是Linux更加流行)。
`为什么Linux如此成功呢?`原因有4个方面,而且它们都与Linus本人相关。

- 首先,Linus Torvalds是一名技能和知识极端丰富的程序员。他是一名高效的工人,而且他热爱程序。换句话说,他正好是那种准各自己编写操作系统内核的人。
- 其次,Linus是一名无止境的完美主义追求者。Linus致力于他的工作,当遇到问题时,他会放弃休息,直至找到合适的解决方案。在Linux的早期,Linus快速地对内核进行修改,有时候甚至每天发行一个新内核,这很不寻常。
- 第三,Linus拥有令人喜爱的个性。人们描述他是一个低调、谦逊的家伙,一个诚实的高尚人物(参见图)。无论是当面还是在线,Linus与他人相处得都非常融洽。正如他在一次采访中的评述:“与Richard Stallman不同,我真的没有什么要说的。”
- 第四,也是最重要的一点,就是Linus拥有使用Internet的天分,可以将编程天才的智慧通过Internet融合在一超:成千上万人志愿参与修改及扩展Linux内核。

![Linus Torvalds](../post_img/58f3521bab64415134001d06)

从一开始,Linus就尽可能快地发布新版本的内核。通常,程序员都喜欢持有新版本的软件,这样他们可以彻底地测试它,并且在程序向大众发布之前尽叮能地修复各种bug。
Linus的天才在于他意识到,由于他将软件发行给大量的爱好者,因此可以有许多的人来测试以及阅读新代码,致使bug很快就被发现。这一思想收录在所谓的Linus法则中:“Given enough eyeballs,all bugs are shallow(让足够多的人阅读源代码,错误将无所遁形)。”
Linus发行新版本内核的速度比以往任何人都要快,bug也会很快被确认和修复——通常是在数小时内。这样的结果使Linux飞快地发展和完善,比历史上任何一个主要软件项目都要快。

----------


> 名称含义
**Linux**
Linux的正确发音方式要有*Bin'-ex*押韵。
从一开始,Linus Torvalds就非正式地使用名称Linux,**Linux**是**Linus,Minux**的缩写,但是.当Linus发布第一个公开版本的内核时,他实际上计划使用名称**Freax**(free Unix)。
当Linus发行第一版的内核时,另一名程序员Ari Lemmke,说服Linus将文件上传到一个由Lemmke运营的服务器上,从而使编程爱好者可以通过一个称为**anonymous FTP**的系统方便地访问文件。Lemmke对名称Freax不满意,因此,当他在创建存放这些文件的目录时,他将这个目录命名为Linux,Linux名称由此而来。

`Linux`具有两层含义：

- Linux指一个内核，无数在Linux项目中工作的程序员的一个产品
- Linux指任何基于Linux内核的操作系统的名称

大多数Linux发行版都使用自由软件基金会的`GNU实用工具`。基于这一原因,FSF坚持基于Linux的操作系统实际上应该称为`GNU/Linux`。

----------


# 应该使用什么类型的Unix

- 如果使用Unix是因为希望学习他如何运转、如何定制工作环境、编程或者只是为了寻找乐趣，可以使用`Linux`，诸如[Debian](http://distrowatch.com/table.php?distribution=debian)、[LinuxMint](http://distrowatch.com/table.php?distribution=mint)、[Ubuntu](http://distrowatch.com/table.php?distribution=ubuntu)、[Fedora](http://distrowatch.com/table.php?distribution=fedora)、[Manjaro](http://distrowatch.com/table.php?distribution=manjaro)、[OpenSUSE](http://distrowatch.com/table.php?distribution=opensuse)、[Deepin](http://distrowatch.com/table.php?distribution=opensuseon=deepin)、[Solus](http://distrowatch.com/table.php?distribution=solus)、或者[其他](http://distrowatch.com/)。
- FreeBSD非常稳定和可靠,并且基本上即装即用。因此,如果在一家公司工作,而且正在寻找一种极少需要操心的操作系统,那么使用FreeBSD。同理,如果在家里工作,并且希望运行一个服务器(如Web服务器).那么使用FreeBSD。如果系统不支持FreeBSD,可以使用NetBSD。如果对安全非常感兴趣,可以使用OpenBSD。
- 如果希望在Microsoft Windows下运行Unix,那么可以使用一个免费的产品Cygwin。一旦安装了Cygwin,您所需做的全部工作就是打开一个Cygwin窗口,在这个窗口中,所有事情看上去都极像Linux。
- 如果希望使用Unix,但是您希望它运行起来像Windows,那么可以使用Macintosh,并运行OS X。
OS X是Macintosh计算机的操作系统。尽管它拥有类Mac的观感,但是它的内部实际上就是Unix。具体而言,OS X使用基于Mach的微内核、FreeBSD实用工具以及一个名为Aqua的专有GUI。
为了在OS X之下直接访问Unix,您只需打开一个Terminal窗口即可(可以在Applications/Utilities文件夹中找到Terminal)。

----------


> 名称含义
**OS X**
OS X(Macintosh的操作系统)的发音为**O-S-ten**,这一名称是个双关语,前一个Mac操作系统称为OS 9,这个操作系统不是基于Unix的,因此,**X**既代表罗马数字10,又可使您联想到Unix。

----------


# 什么是Linux？什么是Unix？

再次总结：

- Unix是一种多用户、多任务处理的操作系统,它由一个类Unix内核、许多类Unix实用工具以及一个类Unix shell构成
- Linux是任何使用Linux内核的Unix的名称

----------


> 提示
Unix是一组为聪明人准备的工具。

----------


# Reference 

[1] Haeley Hahn. Unix & Linux 大学教程[M]. 张杰良, 译. 北京:清华大学出版社, 2010.1:8-31.