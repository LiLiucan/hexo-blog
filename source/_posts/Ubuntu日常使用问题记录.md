---
title: Ubuntu日常使用问题记录
date: 2019-06-29 22:24:02
categories:  Linux
tags:  Ubuntu及树莓派
---

### 截图快捷键
> 桌面截屏：PrtSc
> 窗口截图：Alt  + PrtSc
>选择截图区域：Shift + PrtSc

---

### Ubuntu声卡没有声音的问题解决方法
```
sudo apt-get remove --purge alsa-base pulseaudio
sudo apt-get install alsa-base pulseaudio pavucontrol
sudo alsa force-reload

reboot
sudo apt-get install pavucontrol
```
运行pavucontrol，在弹出的窗口选择正确的声音输出设备即可

---

###  解决安装Ubuntu和Windows双系统后Windows启动项丢失的问题（先安装Windows后安装Ubuntu）
安装grub2
```
sudo apt-get install grub2-common
```
修复启动项
```
sudo update-grub2
```

---

### 安装Chrome浏览器
```
sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -
sudo apt update
sudo apt install google-chrome-stable
```

---


### terminal代理设置
#### 1.http及https代理
如果只是临时用以下可以直接运行命令：
```
export http_proxy="http://192.168.0.103:8123"
export https_proxy="http://192.168.0.103:8123"
```
如果是每次启动terminal都要使用，可以编辑/etc/environment或者~/.bashrc文件。
以编辑/etc/environgment为例：
```
sudo nano /etc/environment
```
在文件结尾添加如下配置：
```
export http_proxy="http://192.168.0.103:8123"
export https_proxy="http://192.168.0.103:8123"
```

#### 2.apt代理
编辑/etc/apt/apt.conf，若该文件不存在则新建
```
sudo nano /etc/apt/apt.conf
```
添加如下配置：
```
Acquire::http::Proxy "http://192.168.0.103:8123";
```

---


### screen的安装及使用
#### 1.安装
```
sudo apt-get install screen
```
#### 2.常用命令及快捷键
创建一个window:
```
screen -S window-name
```
解除绑定 screen session（ screen session会继续运行）:
```
ctr + a + d
```
重新使用之前创建的screen session:
```
screen -r window-name
```
查看screen session列表及当前正在适用的screen session状态:
```
screen -ls
```






