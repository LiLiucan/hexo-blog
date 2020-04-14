---
title: Ubuntu18.04安装PostgresQL
date: 2019-09-09 22:55:57
tags:
issueNumber: 4
---

## 安装 PostgresQL

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

## 确认安装成功

```bash
sudo -u postgres psql -c "SELECT version();"
```

## 进入 PostgresQL 命令行

切换用户

```bash
sudo su postgres
```

进入交互模式

```bash
psql
```

以上两条命令可以合为一条

```bash
sudo -u postgres psql
```

退出

```bash
\q
```

### 创建新的角色（用户）

- 如果当前系统登录用户是 postgres,输入以下命令：

```bash
sudo su - postgres -c "createuser john"
```

- 否则输入以下命令：

```bash
sudo su - postgres -c "createdb johndb"
```

- 将新创建的数据权限赋予指定的数据库用户

打开 PostgreSQL shell

```bash
sudo -u postgres psql
```

运行如下命令：

```bash
grant all privileges on database johndb to john;
```

- 修改用户密码

打开 PostgreSQL shell

```bash
sudo -u postgres psql
```

运行如下 sql 命令：

```bash
ALTER USER postgres WITH PASSWORD ‘postgres’;
```

## 开启远程访问

- 编辑配置文件

打开 postgresql.conf，在 CONNECTIONS AND AUTHENTICATION 区域添加 listen_addresses = '\*'

```bash
sudo vim /etc/postgresql/10/main/postgresql.conf
```

示例：

```bash
#------------------------------------------------------------------------------
# CONNECTIONS AND AUTHENTICATION
#------------------------------------------------------------------------------

# - Connection Settings -

listen_addresses = '*'     # what IP address(es) to listen on;

```

- 保存配置并重启服务

```bash
sudo service postgresql restart
```

- 确认修改是否成功

```bash
ss -nlt | grep 5432
```

输出结果示例：

```bash
LISTEN   0         128                 0.0.0.0:5432             0.0.0.0:*
LISTEN   0         128                    [::]:5432                [::]:*
```

- 配置服务器接受远程连接

编辑 pg_hba.conf 文件

```bash
/etc/postgresql/10/main/pg_hba.conf
```

配置示例：

```
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# The user jane will be able to access all databases from all locations using a md5 password
host    all             jane            0.0.0.0/0                md5

# The user jane will be able to access only the janedb from all locations using a md5 password
host    janedb          jane            0.0.0.0/0                md5

# The user jane will be able to access all databases from a trusted location (192.168.1.134) without a password
host    all             jane            192.168.1.134            trust
```
