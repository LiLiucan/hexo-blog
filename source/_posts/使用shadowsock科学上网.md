---
title: 使用shadowsock科学上网
date: 2019-07-06 00:41:00
categories: 常用服务配置
tags:  翻墙
issueNumber: 6
---

# 非VPS翻墙
推荐使用板瓦工的[justmysocks](http://justmysocks1.net)服务，这货默认会给5个ip，被封还可以换，只是有些贵。与VPS的差别就是这货只能翻墙，VPS还可以用来搭一些服务。

# VPS翻墙解决方案
[VPS翻墙方案架构图](https://github.com/LiLiucan/drawio-charts/blob/master/shadowsocks.png)
我目前是使用vps搭建shadowscoks服务，局域网设备通过代理到安装有shadowsocks客户端的树莓派翻墙，移动设备可以直接安装shadowsocks客户端翻墙，也可以使用树莓派代理。

关于VPS，我个人用过[板瓦工](https://bwh88.net)和[vultr](https://vultr.com)两个服务。搬瓦工的好处是有便宜的机器，缺点是如果ip被封，换ip的话是需要付费的。当然也可以不换ip那就有可能要等俩月看ip会不会解封了。vultr的特点：以充值的方式付费，不使用是不扣费的；可以添加多个VPS实例，当然也是按每个VPS实例收费；多个实例对于翻墙的好处就是不怕封ip，ip被封就放弃当前实例，在添加一个就行了。但是vultr相对搬瓦工来说还是要贵些。

# 服务器配置，系统环境为Ubuntu
服务端需要安装shadowsocks server,之前安装shadowsocks主要有python安装和使用apt安装两种方式，但是目前使用pip安装的方式已失效，apt安装在部分Ubuntu系统版本上有效。所以，我推荐使用docker安装或者使用snap安装。

## 使用docker安装shadowsocks
参考：https://github.com/LiLiucan/docker-shadowsocks
docker的安装和使用自行学习，这里不再介绍

## 使用snap安装shadowsocks
1. 安装snap
```shell
$ sudo apt install snap
```

2. 安装shadowsocks
```shell
$ sudo snap install shadowsocks --devmode # 如果不加--devmode在开启服务的时候使用配置文件的话，会报permission denied错误
```

### 编辑配置文件
- 创建配置文件: /etc/shadowsocks.json
```shell
$ vim /etc/shadowsocks.json
```
- 添加如下配置（2选1）:
1. 单用户配置
```json
{
    "server":"x.x.x.x",  // 这里修改为自己的ip
    "server_port":3389, // 可以设置为其他端口,3389是windows远程桌面端口,一般不会被封
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword", // 这里修改为自己的密码
    "timeout":300,
    "method":"aes-256-cfb" // 可以使用其他的加密方式
}

```
2. 多用户配置
只需将上述配置中的server_port替换为port_password，最终配置如下：

```json
{
    "server":"x.x.x.x",
    "port_password": {
        "8080": "passwd1",
        "8081": "passwd2"
    },
    "local_address": "127.0.0.1",
    "local_port":1080,
    "timeout":300,
    "method":"aes-256-cfb"
}

```

### 开启shadowsocks服务
```shell
$ sudo snap run shadowsocks.ssserver -c /etc/shadowsocks.json -d start
```

### 关闭shadowsocks服务
```shell
$ sudo snap run shadowsocks.ssserver -c /etc/shadowsocks.json -d stop
```

# 树莓派客户端

### 客户端安装配置

客户端的安装和使用方式和服务端一样，配置和服务端单用户配置一样。

### 开启shadowsocks客户端
```shell
$ sudo snap run shadowsocks.sslocal -c /etc/shadowsocks.json -d start
```

### 关闭shadowsocks客户端
```shell
$ sudo snap run shadowsocks.sslocal -c /etc/shadowsocks.json -d stop
```

### 使用polipo实现socks5转http代理
- 安装 polipo
```shell
$ apt-get install polipo
```

- 编辑配置文件
```shell
$ vim /etc/polipo/config

添加如下内容：
socksParentProxy = "127.0.0.1:1080" // 此处的1080端口对应服务端配置的local_port
socksProxyType = "socks5"
proxyAddress = "127.0.0.1"	
proxyPort = 8123 // 暴露的代理服务器端口
```

### 局域网客户端代理配置
局域网客户端将网络代理设置为上述树莓派客户端即可，代理端口号为上述proxyPort。
