title: Fedora配置shadowsock
date: 2017-01-26 15:24:47
updated: 2017-05-24 22:52:36
categories:
- Study
- Computer
- Tools
---
## Fedora配置shadowsock-qt5

### 下载

添加Copr源

```s
$ sudo dnf copr enable librehat/shadowsocks
```

下载`shadowsocks-qt5`并进行安装：
```sh
$ sudo dnf install shadowsocks-qt5
```

### 配置

打开软件，出现如下界面：
![](../post_img/58cd4da8ab6441359b000a6b)
依次打开`File->Import Connections from gui-config.json`，导入相应的json文件， 出现以下画面：
![](../post_img/58cd4da8ab6441359b000a6d)
但此时还不能真正的`翻墙`，还需要进行代理设置

### 代理设置

打开`Setting->Network->Network proxy`，设置`Method`为`Manual`，并在`Socks Host`设置为`127.0.0.1`,端口设置为`1080`(视json配置而定)，如下所示：
![](../post_img/58cd4da8ab6441359b000a6a)

### 建立连接

建立连接之前，可以事先测试各个连接的延迟，并选择延迟最短的代理进行连接。建立连接后，便可看到已使用的流量计数，测试google搜索也可通过了。
![](../post_img/58cd4da8ab6441359b000a6c)

### 设置GFW代理配置

为了让代理只是施行其**翻墙**的作用，而不需所有的访问都先到国外走一遭，还需要配置GFW代理配置。教程如下，可自行访问[github](https://github.com/JinnLynn/genpac)获取更多信息。

```sh
# 安装
$ sudo pip install genpac
$ sudo pip install https://github.com/JinnLynn/genpac/archive/master.zip
# 配置GFW 
# 生成pac文件
$ genpac -p "SOCKS5 127.0.0.1:1080" --gfwlist-url="https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt" --output="~/autoproxy.pac"
```

在生成pac文件后，便可以通过该文件达到真正**翻墙**的作用。打开`Setting->Network->Network proxy`，设置`Method`为`Automatic`，并设置pac文件路径`file:///home/xxx/autoproxy.pac`(xxx为用户名)，如下所示：
![](../post_img/58cd4da8ab6441359b000a6e)