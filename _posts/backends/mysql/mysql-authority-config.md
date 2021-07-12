---
title: MySQL 数据库设置远程权限
tags:
  - grant privileges
categories:
  - 后台技术
  - MySQL
keywords:
  - mysql
  - grant all privileges
description: 在 MySQL 中分配给用户不同的访问权限。
abbrlink: '43555396'
author: zucchini
group: notes
date: 2016-04-29 10:55:00
---

## 设置访问单个数据库权限 

-   设置用户名为 root，密码为空，可以访问数据库 test

```shell
mysql>grant all privileges on test.* to 'root'@'%';
```

## 设置访问全部数据库权限 

-   设置用户名为 root，密码为空，可以访问所有数据库

```shell
mysql>grant all privileges on *.* to 'root'@'%';
```

## 设置指定用户名访问权限 

-   指定用户名为 liuhui，密码为空，可以访问所有数据库

```shell
mysql>grant all privileges on *.* to 'liuhui'@'%';
```

## 设置密码访问权限 

-  设置用户名为 liuhui，密码为 liuhui，可以访问所有数据库

```shell
mysql>grant all privileges on *.* to 'liuhui'@'%' IDENTIFIED BY 'liuhui';
```

## 设置指定可访问主机权限 

-   设置用户名为 liuhui，密码为 liuhui，可以访问所有数据库，只有 10.1.1.1 这台机器有权限访问

```shell
mysql>grant all privileges on *.* to 'liuhui'@'10.1.1.1';
```

## 设置对应的密码级别 

###   参数解释

```shell
mysql>show variables like 'validate_password%';
```

-   `validate_password_dictionary_file`: 用于难密码强度的字典文件路径
-   `validate_password_length`: 密码最小长度，参数默认为 8，
-   `validate_password_mixed_case_count`: 密码至少要包含的小写字母个数和大写字母个数
-   `validate_password_number_count`: 密码至少要包含的数字个数
-   `validate_password_policy`: 密码强度难 0/LOW 1/MEDIUM 2/STRONG
-   `validate_password_special_char_count`: 密码至少要包含的特殊字符数


