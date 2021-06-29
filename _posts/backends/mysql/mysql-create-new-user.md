---
title: 新增 MySQL 用户
tags:
  - create user
  - set password
categories:
  - 后台技术
  - MySQL
keywords:
  - mysql
  - create user
  - flush privileges
description: 在 MySQL 中新增用户。
abbrlink: 5688b61f
date: 2018-07-12 07:17:00
---

- 创建本地用户

```sql
create user 'test'@'localhost' identified by 'password';
```


-   创建局域网用户

```sql
create user 'test'@'%' identified by 'password';
```


-   刷新

```sql
flush privileges;
```


-   修改密码

```sql
set password for 'test'@'localhost' = password('newpassword');
```


如果是当前用户：

```sql
SET PASSWORD = PASSWORD("newpassword");
```


-   授权

授权相关操作见: [MySQL 数据库设置远程权限](/notes/backends/mysql/mysql-authority-config)

这里补充一下 MySql 移除权限的命令：

```sql
REVOKE privilege ON databasename.tablename FROM 'username'@'localhost';
```


- 删除用户

```sql
drop user 'username'@'localhost'
```

