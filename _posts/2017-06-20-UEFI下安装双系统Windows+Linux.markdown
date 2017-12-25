title: UEFI下安装双系统Windows+Linux
date: 2017-06-20 21:22:24
updated: 2017-06-20 21:55:08
tags:
- Linux
categories:
- Study
- Computer
- Tools
---
> 在UEFI下，相信在安装双系统时会遇到一个问题：先有windows再装Linux，会发现没有出现Linux的引导项而是直接进入Windows。本文主要介绍解决方法，在宏碁TMTX50测试通过。

# 安装前准备

## 关闭Secure Boot

**Secure Boot**用于让UEFI固件只加载被签名的引导程序，这可以让固件安全的只加载现有的被签名的Windows引导程序，却让安装Linux造成了很大困扰，而这也是直接导致重启后直接进入Windows的原因。所以，首先需要关闭**Secure Boot**，进入**BIOS**后找到该选项并**Disabled**即可。宏碁(其他品牌电脑可能也是)比较狗血，需要设置**BIOS**密码后才能进行**Disabled**操作，所以先设置密码后，就可以了。
此外，还可以设置在启动时按**F12**选择进入不同的引导程序(在第一次安装后发现直接进入Windows后再重启就可以用此方法去找回Linux的引导程序尔后进入Linux)。

## 更新grub引导

在安装完Linux或者第二次进入Linux之后，可以更新**grub**引导程序，默认安装了**Grub2**的Linux(如Ubuntu)，可以执行`update-grub`命令(update-grub与update-grub2是一样的，后者是软连接，指向前者)，但所有Linux系列都可以执行以下命令：

```
$ sudo grub-mkconfig -o /boot/grub2/grub.cfg
```

实际上，`update-grub`也是执行以上命令。
在更新**grub**之后，Linux的引导程序会自动找到Windows的引导程序并将其添加进引导项中。

## 查看.efi文件位置

在以UEFI形式安装Linux之后，**boot**分区中的EFI文件中会增加一个文件夹，名称取决于安装的Linux发行版，如我此次安装的是`deepin`，则文件名称为**deepin**。打开文件夹发现其中有一个命名为**grubx64.efi**的文件，其为启动Linux的引导文件，只要将此文件替换掉启动Windows的引导文件，则可以在开机的时候进入Linux了。

## 修改指定.efi文件

步骤如下：

1. 重启进入Windows
2. 进入管理员命令行:按`win+x`，再按`a`，或者用其他方式皆可
3. 修改指定efi文件，输入命令:`bcdedit /set {bootmgr} path \EFI\deepin\grubx64.efi`

至此，双系统问题解决完毕。重启后即可看到Linux引导项。

# Reference

[1] [Linux与Windows 10用grub引导教程](http://www.jianshu.com/p/5007e555ec12)
[2] [What is difference between update-grub and update-grub2?](https://askubuntu.com/questions/167763/what-is-difference-between-update-grub-and-update-grub2)
[3] [GRUB2](https://fedoraproject.org/wiki/GRUB_2)