title: Fedora 25 安装XMind
date: 2017-04-26 16:17:32
updated: 2017-05-07 16:02:17
tags:
- Tool
categories:
- Study
- Computer
- Tools
---
## 前述

`XMind`是一款非常实用的商用思维导图软件，软件可扩展、跨平台、稳定性和性能强。可以有效地帮助用户进行头脑风暴及进行组织管理等活动，提高生产力。

## 下载

点击官网[下载地址](http://www.xmind.net/downloads/)，再选择下载即可。下载后无需安装，所有所需文件皆在压缩包内。此时本人下载的压缩包名称为`xmind-8-update1-linux.zip`，表示为第8版。

## 解压

解压后，会有64位版本`XMind_amd64`及32位版本`XMind_i386`。根据系统需要选择不同的版本，进入相应文件夹后可执行`XMind`尝尝鲜。

> 提示
由于`XMind`采用Java语言开发，故需要有`Jre`支持，可通过安装Java实现。
```sh
$ sudo dnf install java
```

## 移动程序

为了Linux软件管理的一致性，本文将程序移动至`/opt/`目录中，并为程序顶层目录更名为`/opt/xmind/`。

```sh
$ mv xmind-8-update1-linux/ xmind/
$ sudo mv xmind/ /opt/
```

## 添加软连接

为了方便执行程序，需要为可执行程序添加软连接，如下：

```sh
$ sudo ln -s /opt/xmind/XMind_amd64/XMind  /usr/local/xmind 
```

## 修改配置文件

配置文件`XMind.ini`中包含了程序启动时的相关信息，而在默认时使用的是相对路径来调用某些插件等。为了在各个环境下都能执行程序，需要将可执行程序转换为绝对路径。

> 提示
程序的主目录为`/opt/xmind/`。

更改如下：

```ini
-configuration
/opt/xmind/XMind_amd64/configuration
-data
/opt/xmind/workspace
-startup
/opt/xmind/plugins/org.eclipse.equinox.launcher_1.3.200.v20160318-1642.jar
--launcher.library
/opt/xmind/plugins/org.eclipse.equinox.launcher.gtk.linux.x86_64_1.1.400.v20160518-1444
--launcher.defaultAction
openFile
--launcher.GTK_version
2
-eclipse.keyring
@user.home/.xmind/secure_storage_linux
-vmargs
-Dfile.encoding=UTF-8
```

此时，已可以在终端任何工作目录下启动程序了。

## 添加图标

为了可以在桌面端点击图标启动`XMind`，可以编写`desktop`文件，将其存于`/usr/share/applications/xmind.desktop`，文件内容如下：

```desktop
Name=XMind
Encoding=UTF-8
Comment=XMind
Exec=/usr/bin/xmind
Icon=/usr/share/icons/la-capitaine-icon-theme-0.4.0/apps/scalable/XMind.svg
Terminal=false
StartupNotify=true
Type=Application
Categories=Application;
```

其中，`Icon`标识为图标文件路径，可根据自身需要进行修改。

## 破解

测试 `XMind7`、`XMind8`、`XMind8 Update1` 通过。

1. 下载[XMindCrack.jar](https://pan.baidu.com/s/1bpist2n)
2. 将`XMindCrack.jar`放在`安装目录/plugins`下，如`/opt/xmind/plugins/XMindCrack.jar`
3. 编辑`XMind.ini`，在最后追加`-javaagent:/opt/xmind/plugins/XMindCrack.jar`
4. 断开网络, 或者使用防火墙阻止 XMind 联网, 或者在 `/etc/hosts`中添加`0.0.0.0 www.xmind.net`
5. 打开`xmind`，输入邮箱、序列号，邮箱任意，序列号如下：

```code
XAka34A2rVRYJ4XBIU35UZMUEEF64CMMIYZCK2FZZUQNODEKUHGJLFMSLIQMQUCUBXRENLK6NZL37JXP4PZXQFILMQ2RG5R7G4QNDO3PSOEUBOCDRYSSXZGRARV6MGA33TN2AMUBHEL4FXMWYTTJDEINJXUAV4BAYKBDCZQWVF3LWYXSDCXY546U3NBGOI3ZPAP2SO3CSQFNB7VVIY123456789012345
```
激活即可。

> 提示
如果因为程序联网后，导致之前的序列号注册无效，则仍然可以使用该序列号，只需要更换邮箱即可。
切记勿让程序联网。

## Reference

[1] [LINUX fedora 安装XMIND 思维导图](http://www.07net01.com/linux/2016/11/1716975.html)
[2] [官方下载链接](http://www.xmind.net/download/)
[3] [xmind百度百科](http://baike.baidu.com/item/XMIND)
[4] [昔日风xMind破解](http://www.jianshu.com/p/254392b4f879)