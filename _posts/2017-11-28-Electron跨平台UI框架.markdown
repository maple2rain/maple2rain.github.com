---
layout: post
title: Electron跨平台UI框架
date: 2017-11-28 14:51:34
updated: 2017-11-29 17:57:04
categories: Electron
excerpt: 本文初步介绍`Electron`为何物，并且介绍部分环境搭建及后续打包等操作。![](../post_img/5a1d20beab64416ff30029b6)
tags:
- Electron
---

* content
{:toc}

# 前言

鉴于在**Windows**进行桌面应用开发使用**QT**有许多不方便的地方，所以大致了解下`Electron`。

![](../post_img/5a1d20beab64416ff30029b6)

而基于`Electron`开发的比较成熟的产品有：`Atom`，`Visual Studio Code`等。

![](../post_img/5a1d20beab64416ff30029b8)

# 什么是Electron


## 概述

`Electron`前称是`atom shell`，从github开源项目`Atom`编辑器中抽离出来，是一个能让你通过`JavaScript`、`HTML` 和`CSS` 构建桌面应用的**库**。这些应用能打包到 Mac、Windows 和 Linux 电脑上运行，当然它们也能上架到 Mac 和 Windows 的 app stores。

## 组成

`Electron`结合了`Chromium`、`Node.js`和用于调用操作系统本地功能的`API`如打开文件窗口、通知、图标等）。

![](../post_img/5a1d20beab64416ff30029b7)

- `Chromium` : Google 创造的一个开源库，并用于 Google 的浏览器 Chrome
- `Node.js` : 一个用于在服务器运行JavaScript的工具，拥有文件系统和网络的权限
- `Native API` : 支持3种操作系统（Windows、Mac和Linux）的原生API库

## 开发体验

- 基于`Electron`的开发，就好像开发一个网页一样，而且能够无缝地使用`Node`(使用内置Node及托管在`npm`上的模块)
- 并通过`HTML`和`CSS`构建界面
- 只需为一个浏览器(最新的 Chrome)进行设计(由于并非所有浏览器都提供同样的样式，导致web开发需要进行很多适配，而在此没有这个问题)

# 渲染过程

## 两个进程

Electron 有两个种进程：**主进程**和**渲染进程**。
有些模块会工作在其中一个进程上，而有些会在两个进程之上。主进程更多地充当幕后角色，而渲染进程则是应用的每个窗口(window)。

## 主进程

主进程，通常是一个命名为`main.js`的文件，该文件是每个 `Electron`应用的入口。它控制了应用的生命周期（从打开到关闭）。它能调用原生元素和创建新的（多个）渲染进程，而且整个`Node API`是内置其中的。

![](../post_img/5a1d247fab64416dcc002ae3)

- 调用原生元素：打开 diglog 和其它操作系统交互均是资源密集型操作（出于安全考虑，渲染进程是不能直接调用本地资源的），因此都需要在主进程完成（不涉及渲染进程）。

## 渲染进程

渲染进程是应用的一个浏览器窗口。与主进程不同，它能存在多个（一个 Electron 应用只能有一个主进程）并且是 **相互独立**的，它们也能**隐藏**的。
它通常被命名为`index.html`。它们就像典型的 HTML 文件，但在 Electron 中，它们能获取完整的 Node API 特性。因此，这也是它与其它浏览器不同的地方。

## 举个例子

![](../post_img/5a1d247fab64416dcc002ae4)

## 相互通信

尽管主进程和渲染进程都有各自的任务，但它们之间也有需要协同完成的任务。因此它们之间需要通讯。
**IPC**就为此而生，它提供了进程间的通讯。主进程和渲染进程都有一个**IPC**模块。利用它能在主进程与渲染进程之间传递信息。

![](../post_img/5a1d247fab64416dcc002ae5)

# 总结

`Electron`应用就像`Node`应用，它也使用一个**package.json**文件 。该文件能定义哪个文件作为主进程，并因此让 Electron 知道从何启动你的应用。然后主进程能创建渲染进程，并能使用 IPC 让两者间进行消息传递。

## package.json

```json
{
  "name": "electron-quick-start",
  "version": "1.0.0",
  "description": "A minimal Electron application",
  "main": "main.js",
  "scripts": {
    "start": "electron ."
  },
  "repository": "https://github.com/electron/electron-quick-start",
  "keywords": [
    "Electron",
    "quick",
    "start",
    "tutorial",
    "demo"
  ],
  "author": "GitHub",
  "license": "CC0-1.0",
  "devDependencies": {
    "electron": "~1.7.8"
  }
}

```

![](../post_img/5a1d26f4ab64416ff3002bbb)

# 环境配置

## 安装`Node.js`及`npm`

- 资源下载

    进入[Node官网](https://nodejs.org/zh-cn/download/)，安装相应系统的版本。

- 安装配置

    对于windows，点击安装包安装完成后，环境变量会自动将安装目录添加到`Path`中：
    ![](../post_img/5a1d26f4ab64416ff3002bba)
    新版的`Node.js`已经集成`npm`，因而无需重复安装。
    对于Mac或Linux，可以直接使用命令直接安装，如**Ubuntu**下为:
    
```sh
$ sudo apt install nodejs
$ sudo apt installl npm
```

- 版本查询
    假如安装成功，结果如下：
    ![](../post_img/5a1d27c3ab64416dcc002bb7)

# 快速起步

## GitHub仓库

`Electron`仓库地址为:[electron](https://github.com/electron/electron)

## 使用国内镜像安装

设置代理为国内地址，通常使用阿里云提供的淘宝镜像:https://npm.taobao.org/或者腾讯的镜像:https://gems.ruby-china.org/，安装某一`package`时使用代理地址： 

```sh
npm install -g package --registry=https://registry.npm.taobao.org
```

为了不用每次安装都指定一个地址，可以直接安装淘宝定制的`cnpm`，如下：

```sh
//安装cnpm
npm install -g cnpm --registry=https://registry.npm.taobao.org
//使用cnpm安装package，例如electron，-g选项为全局
cnpm install -g electron
```

## 起步案例

github上提供了一个简单的案例：[electron-quick-start ](https://github.com/electron/electron-quick-start)
根据官方引导，使用以下指令：

```
# Clone this repository
git clone https://github.com/electron/electron-quick-start

# Go into the repository
cd electron-quick-start

# Install dependencies
cnpm install

# Run the app
cnpm start

# or
npm start

# or
electron .
```

生成应用如下，展示的界面其实就是electron-quick-start目录下index.html的布局界面。
![](../post_img/5a1d2d4dab64416ff3002d49)

## Demo

[官网](https://electronjs.org/)
[Demo下载链接](https://github.com/electron/electron-api-demos/releases)

![](../post_img/5a1d3f72ab64416ff3003108)


# 使用VS Code开发

使用`VS Code`打开目录，结果如下：

![](../post_img/5a1d34b3ab64416dcc002eb0)

## 进行调试

默认的调试器没有添加`runtimeExecutable`字段，且为正确使用调试协议，故而可以自定义一个调试方案，添加配置，如下：

1. 添加配置
    ![](../post_img/5a1d34b2ab64416dcc002ead)
2. 选择调试执行程序

    ![](../post_img/5a1d34b2ab64416dcc002eae)
    
3. 修改调试协议

    ```sh
    {
        "type": "node",
        "request": "launch",
        "name": "Electron Main",    // 调试器名称
        "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/electron",
        "program": "${workspaceFolder}/main.js",    // 调试主进程
        "protocol": "inspector"     // 调试协议
    }
    ```

4. 成功设置后即可设置断点进行调试
![](../post_img/5a1d34b2ab64416dcc002eaf)

# 打包发布

## 打包

专门的打包工具为`electron-packager`，首先需要进行全局安装:

```sh
npm install electron-packager -g
```

之后就可以执行打包，如：
```sh
electron-packager . app --out ../app_dir --version=1.0.0
```

这段语句表示的意思是把当前文件目录下的资源（.）命名为`app`打包到父级的`app_dir`文件夹，并且指定版本为`1.0.0`。

此时`electron-packager`就会自动判别当前的操作系统打包对应的文件，例如windows系统下就会打包成.exe格式。

如果想一次性把所有的操作系统都打包一遍，可以在上面打包语句后面加上`-all`。

但是实际上，打包时会默认将很多不必要的包都添加进去。为了排除那些，可以指定`--ignore`选项。
因此，一般的打包命令如下：

```sh
electron-packager <sourcedir> <appname> --out=<outputdir> --overwrite --ignore=node_modules/electron-* --ignore=node_modules/.bin --ignore=.git --prune
```

- `<sourcedir>` : 项目根目录
- `<appname>` : 生成的app名称
- `--ignore=` : 指定要忽略的内容
- `--prune` : 删除dev依赖性

例如：

```sh
electron-packager . app --out=../app/ --overwrite=true --ignore=node_modules/electron-* --ignore=node_modules/.bin --ignore=.git --ignore=dist --prune
```

也可将该命令写在`package.json`的`scripts`字段里，如`package`命令，如：

```sh
"scripts": {
    "package": "electron-packager . appn  --version=0.36.4 --out=../appn/ --overwrite=true --ignore=node_modules/electron-* --ignore=node_modules/.bin --ignore=.git --ignore=dist --prune"
}
```

此时的打包命令为`npm run package`。

## 加密资源

尽管完成了打包功能，但实际上所有代码会在每个`resources`文件夹里的`app`文件夹里，代码暴露无遗，因此需要使用加密功能，在此使用`asar`包。

首先进行安装：

```sh
cnpm install asar -g
```

安装完成后，则可使用`asar`命令对`app`文件夹进行打包，如下：
```sh
asar pack <dir>/app app.asar
```

执行完毕后就生成了`app.asar`文件，之后就可以删除`app`文件夹。
**特别提醒**: 需要注意的是，加密生成的文件必须是`app.asar`；而如果发现总是第一次生成的程序，则把`resources`目录下的`app.asar`删除重新生成即可。

## 使用NSIS制作安装程序

这个就hin简单了。

# 引用

[1] [Electron的本质](http://www.open-open.com/lib/view/open1479349380061.html)

[2] [使用electron构建跨平台Node.js桌面应用经验分享](http://www.zhangxinxu.com/wordpress/2017/05/electron-node-js-desktop-application-experience/)

[3] [Electron文档](https://electronjs.org/docs)

[4] [Electron教程](https://www.w3cschool.cn/electronmanual/wcx31ql6.html)

[5] [Electron开发桌面应用](https://github.com/pfan123/electron-docs)
