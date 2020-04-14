---
title: Ubuntu16之后的版本配置开机启动
date: 2019-11-27 23:30:57
categories:  Linux
tags: 日常配置
issueNumber: 
---

### 创建rc-local.service文件
```sh
sudo vim /etc/systemd/system/rc-local.service
```

### 编辑之后的rc-local.service文件内容如下：
```
[Unit]
Description=/etc/rc.local Compatibility
ConditionPathExists=/etc/rc.local
 
[Service]
Type=forking
ExecStart=/etc/rc.local start
TimeoutSec=0
StandardOutput=tty
RemainAfterExit=yes
SysVStartPriority=99
 
[Install]
WantedBy=multi-user.target
```

### 创建rc.local文件
```sh
sudo vim /etc/rc.local
```

### rc.local文件内容如下：
```
#!/bin/sh -e
echo "测试命令。" > /usr/local/test.log
exit 0
```

### 为rc.local添加执行权限
```sh
sudo chmod +x /etc/rc.local
sudo systemctl enable rc-local
```

### 启动服务并检查服务状态
```sh
sudo systemctl start rc-local.service
sudo systemctl status rc-local.service
```

### 重启，启动成功后查看test.log，若文件存在并有上述内容，表示启动脚本配置成功。查看test.log命令如下：
```
cat /usr/local/test.log
```