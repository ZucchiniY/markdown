---
title: select into 和 insert into select
tags:
  - select into
  - insert into select
categories:
  - 后台技术
  - MySQL
description: 在 MySQL 中使用 insert into ... select 方法。
abbrlink: 6be6380e
date: 2018-08-07 06:21:00
---


## SELECT INTO 

`SELECT INTO` 语句从一个表复制数据，然后把数据插入到另一个表中。

> MySQL 是不支持 `select ... into` ，但是可以使用 `insert into ... select`，当然也可以使用 `create table <new table> select * from <old tabel>`

可以复制所有的列插入到新表中:

```sql
select *
into newtable [in externaldb]
from table
```

或者复制希望的列到新表中:

```sql
select column_name(s)
into newtable [in externaldb]
from table;
```


## INSERT INTO SELECT 

这个命令可以从一个表复制到另一个表。这个表之前的数据对最后的结果不会有影响。

同 `select ... into` 一样，可以所有列也可以指定列。

所有数据：

```sql
insert into table2
select * from table1;
```

指定列：

```sql
insert into table2
(solumn_name(s))
select column_name(s)
from table1;
```
