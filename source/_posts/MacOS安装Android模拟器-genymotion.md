---
title: MacOS安装Android模拟器--Genymotion
date: 2019-11-27 23:30:57
categories: Android
tags: 日常配置
issueNumber:
---

### 1. [下载并安装 VirtualBox-5.1.2](https://link.jianshu.com/?t=http://download.virtualbox.org/virtualbox/5.1.2/VirtualBox-5.1.2-108956-OSX.dmg。

> 注意：一定要安装 5.1.2 版本的 VirtualBox，否则使用 Genymotion 运行安卓模拟的时候会报错

### 2. [下载并安装 Gnymotion](https://www.genymotion.com/download/)

> 注意：需要注册后下载安装

### 3. 可能遇到的问题

#### 使用 Genymotion 安卓模拟器安装应用的时候报‘INSTALL_FAILED_NO_MATCHING_ABIS’错误

解决方法：
1、下载 Genymotion-ARM-Translation_v1.1.zip；可以到官网下载 ，如果嫌速度太慢也得可以到百度云；
2、.运行 Genymotion，并 start 你配置好的 virtual device
3、将下载好的 Genymotion-ARM-Translation_v1.1.zip 拖拽到 Genymotion 模拟器里面
4、重启模拟器

#### 使用 Genymotion 安卓模拟器安装应用的时候提示不允许未知来源应用安装

解决方法：
进入‘设置’，勾选‘未知来源’选项
