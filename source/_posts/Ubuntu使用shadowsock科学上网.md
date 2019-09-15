---
title: Ubuntu系统环境下shadowsock安装和使用
date: 2019-07-06 00:41:00
categories: 常用服务配置
tags:  翻墙
issueNumber: 6
---

# 整体解决方案
[翻墙方案架构图](https://github.com/LiLiucan/drawio-charts/blob/master/shadowsocks.png)
我目前是使用vps搭建shadowscoks服务，局域网设备通过代理到安装有shadowsocks客户端的树莓派翻墙，移动设备可以直接安装shadowsocks客户端翻墙，也可以使用树莓派代理。

关于vps,我之前用过[板瓦工](https://bwh88.net)，但是ip被封了，换ip要收费，遂放弃。后来用过板瓦工的[justmysocks](http://justmysocks1.net)服务，这货默认会给5个ip，被封还可以换，但是感觉还是有点贵，亦放弃。后来还是用了[vultr](https://vultr.com)的vps。优点是最近有活动，新用户注册即送50美元，充值10美元还有优惠，用3.5美元/月的机器，这样基本上花60人民币可以用一年多。缺点是日本 美国 新加坡节点基本上没有还没被墙的ip，只好用欧洲节点，我用的是德国的节点，用起来还好。

# 服务器配置

###  安装python-pip
```shell
$ apt install python-pip
或
$ apt install python3-pip
```


### 使用pip3安装shadowsocks
- 安装
```shell
$ pip install https://github.com/shadowsocks/shadowsocks/archive/master.zip
或
$ pip3 install https://github.com/shadowsocks/shadowsocks/archive/master.zip
```

- 确认是否安装成功
```shell
$ ssserver --version
```

### 编辑配置文件
- 创建配置文件: /etc/shadowsocks.json
```shell
$ vim /etc/shadowsocks.json
```
- 添加如下配置:
```json
{
    "server":"45.32.156.231",  // 这里修改为自己的ip
    "server_port":3389, // 可以设置为其他端口,3389是windows远程桌面端口,一般不会被封
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword", // 这里修改为自己的密码
    "timeout":300,
    "method":"rc4-md5" // 可以使用其他的加密方式
}

```
- 多用户配置
只需将上述配置中的server_port替换为port_password，最终配置如下：

```json
{
    "server":"45.32.156.231",  // 这里修改为自己的ip
    "port_password": {
        "8080": "passwd1",
        "8081": "passwd2"
    },
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword", // 这里修改为自己的密码
    "timeout":300,
    "method":"rc4-md5" // 可以使用其他的加密方式
}

```

### 开启shadowsocks服务
```shell
$ ssserver -c /etc/shadowsocks.json -d start
```


# 树莓派客户端

### 客户端安装配置

客户端安装配置都和服务端一样

###  开启shadowsocks客户端

```shell
$ sslocal -c /etc/shadowsocks.json -d start
```

### 使用polipo实现socks5转http代理
- 安装 polipo
```shell
$ apt-get install  polipo
```

- 编辑配置文件
```shell
$ vim /etc/polipo/config

添加如下内容：
socksParentProxy = "127.0.0.1:1080"
socksProxyType = "socks5"
proxyAddress = "127.0.0.1"	
proxyPort = 8123
```

### 局域网客户端代理配置
局域网客户端将网络代理设置为上述树莓派客户端即可
