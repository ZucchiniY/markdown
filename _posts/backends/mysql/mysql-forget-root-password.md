---
title: MySQL 中三个常见的问题解决
tags:
  - 重置 root 密码
  - 报错 10060
  - 清理连接数
categories:
  - 后台技术
  - MySQL
description: 重置 root 用户密码、解决启动报错 10060、解决超过最大连接数问题。
abbrlink: 53f313a5
date: 2018-09-27 08:01:00
---

## 重置 root 用户密码

因为长时间未使用 MySql 导致忘记了 root 密码，现在将修改 root 用户密码的方法记录下来。 

### 修改配置 

```shell
vi /etc/my.cnf
```

在 **[mysqld]** 中添加 `skip-grant-tables`

例如：

```yml
[mysqld]
skip-grant-tables
datadir=/var/lib/MySQL
socket=/var/lib/mysql/mysql.sock
```


### 重启mysql 

```shell
service mysql restart
```


### 用户无密码登录 

```shell
mysql -uroot -p (直接点击回车，密码为空)
```


### 选择数据库并修改密码 

```sql
use mysql;
update user set authentication_string=password('123456') where user='root';
flush privileges;
```


### 删除并重启 mysql 服务 

这个时候发现，确实可以用新的密码登录了， 但是操作的时候会提示：
`ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.` 。

这是因为少了一步修改导致，执行下面的命令进行修改：

```sql
alter user 'root'@'localhost' identified by 'youpassword';
```

执行的时候发现会提示一个新的报错： `ERROR 1819 (HY000): Your password does not satisfy the current policy requirements` ，经过搜索，发现是因为密码有要求导致，可以选择使用一个包含大小写字母、数字和符号的密码，也可以选择更新一个简单的密码：

```sql
set global validate_password_policy=0;
```

这次密码的问题就彻底解决了。

## 报错 10060 

除了可能是因为用户权限不足外，还有可能是服务器不允许请求 3306 端口，需要在服务器管理中，增加入站规则，允许 3306 端口即可。

具体的位置在：

**服务器管理 => 高级安全 Windows 防火墙 => 入站规则 => 新建规则 => 端口3306 => 允许连接**

## 清理连接数

在管理 MySQL 服务器的过程中，会出现连接时间过长的问题，分析之后发现主要是之前写的操作 MySQL 的程序未正常结束，导致资源占用过高。

这时可以使用以下的方案进行自理：

```sql
show status like 'Threads%';
show variables like '%max_connections%';
show global status like 'Max_used_connections';
show status;
show processlist;
show OPEN TABLES where In_use > 0;
```

这时可以找到真正占用数据库资源的进程，然后使用 `kill <process id>` 结束掉进程。
