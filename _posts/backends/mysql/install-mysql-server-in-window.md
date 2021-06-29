---
title: 在 Windows 上安装 MySQL
tags:
  - MySQL 安装
categories:
  - 后台技术
  - MySQL
keywords:
  - 启动 MySQL 服务
  - install mysql
description: 在 Windows 系统上安装 MySQL 服务并启动。
abbrlink: a0232202
author: zucchini
date: 2020-08-12 22:42:00
---

## 安装 MySQL

从官方网站下载 MySQL 的安装包太慢了，这里推荐从 [清华源](https://mirrors.tuna.tsinghua.edu.cn/mysql/downloads/MySQLGUITools/?C=M&O=D) 上下载，几分钟就能搞定。

下载之后，将 MySQL 安装对应的目录下，然后在其同级目录下建一个新的数据文件夹 **mysqldata** 。


## 配置 MySQL 文件

在 MySQL 的目录下（bin 文件夹的同级目录）下新建一个空白 *my.ini* 文件。

文件内容如下：

```shell
[mysqld] 
# 设置mysql的安装目录，也就是刚才我们安装的目录
basedir=<安装目录>
# 设置mysql数据库的数据的存放目录，刚才创建的mysqldata目录
datadir=<新建目录>
# 设置默认使用的端口
port=3306
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人试图攻击数据库
max_connect_errors=10
# 服务端使用的字符集
character-set-server=utf8mb4
# 数据库字符集对应一些排序等规则使用的字符集
collation-server=utf8mb4_general_ci
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件作为认证加密方式
# MySQL8.0默认认证加密方式为caching_sha2_password
default_authentication_plugin=mysql_native_password
 
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
 
[client]
default-character-set=utf8mb4
port=3306
```

## 初始化 MySQL

1. 启动 **命令提示符（管理员）** 
2. `mysqld --initialize-insecure --user=mysql --console` 初始化 MySQL
3. `msyqld --install` 安装 MySQL 的服务
4. 启动 `net start mysql` 启动 MySQL 服务
   > 注意：在启动服务前需要注意两点：1. 必须使用管理员权限；2. 必须在 mysql/bin 的目录下才能生效，否则会提示 系统错误2

这个时候 MySQL 就已经安装完毕，可以使用了。
