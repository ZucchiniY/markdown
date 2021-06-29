---
title: 字符串按排序和时间戳
tags:
  - CAST
  - CONVERT
  - timestamp
categories:
  - 后台技术
  - MySQL
description: MySQL 将字符串按数据进行排序，定义数据字段的时间戳。
abbrlink: 85f1b411
date: 2018-05-22 11:06:00
---


## 排序

自建了一个表，其中的字段为 **char** 或者 **varchar** 的类型。

我们如果直接进行的排序的话，得到的序列是字符顺序的，即 **1,10,2,20,...**
，但是我们希望得到的是 **1,2,3,4,...** 这种序列，有两种方法可以实现排序。

-   手动转换

```sql
select id from db.sql order by id + 0 desc
```

但是这种方式显得有点丑，其实 Mysql 提供了一个非常好用的函数进行操作。

-   使用函数

**CAST()** 函数和 **CONVERT()** 可以使用。

```sql
select id from db.sql order by CAST(id as SIGNED) desc
```

```sql
select id from db.sql order by CONVERT(id, SIGNED) desc
```

## 时间戳

-   创建新记录和修改现有记录都更新方式

```sql
TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
```

-   创建的时候设置时间，后续的修改不再更新

```sql
TIMESTAMP DEFAULT CURRENT_TIMESTAMP
```

-   创建的时候把字段设置为 0 ，以后修改才更新

```sql
TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
```

-   创建时设置为给定值，以后更新会刷新这个时间

```sql
TIMESTAMP DEFAULT 'yyyy-mm-dd hh:mm:ss' ON UPDATE CURRENT_TIMESTAMP
```
