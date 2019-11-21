---
title: Ubuntu安装PostgresQL
date: 2019-09-09 22:55:57
tags:
issueNumber: 4
---

## 安装PostgresQL
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

## 进入PostgresQL命令行
切换用户
```bash
sudo -i -u postgres
```
进入交互模式
```bash
psql
```
退出
```bash
\q
```

### 创建新的角色（用户）
如果当前系统登录用户是postgres,输入以下命令：
```bash
createuser --interactive
```
否则输入以下命令：
```bash
sudo -u postgres createuser --interactive
```
