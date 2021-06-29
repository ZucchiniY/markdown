---
title: 拼接查询结果中的字符串
tags:
  - concat
  - concat_ws
  - group_concat
  - union
categories:
  - 后台技术
  - MySQL
description: MySQL 里将查询结果按想要的方式整合在一起。
abbrlink: e958b1c9
date: 2018-05-15 10:59:00
---

## CONCAT 

-   将多个结果作为字符串拼接在一起

```sql
concat(str1,str2,...)
```

实例：

```sql
select concat(o.user_name,o.user_number) from user o where user_id = '1'
```

但是如果查询过程中有一个字符串为 **null** 则整个结果都将是 **null** ，这时可以将 **null** 转换为 ''

```sql
    select concat(IFNULL(o.user_name,''),o.user_number) from user o where user_id = '1'
```

如果想将结果分隔，则可以使用下面的方法

```sql
select concat(o.user_name,',',o.user_number) from user o where user_id = '1'
```


但是这种方式显得过于难用，如果字段多了，要写很多将分隔符，这时可以用 **concat_ws** 进行拼接。


## CONCAT_WS 

-   将多个结果拼接在一起，使用指定的分隔符

```sql
concat_ws(separator,str1,str2,...)
```

实例：

```sql
select concat_ws(';',o.user_name,o.user_number) from user o where user_id = '1'
```

这种情况下，结果中有 **null** 的话，也不会返回 **null** ，但是如果将分隔符指定为 **null** 则结果会全变成 **null**

## GROUP_CONCAT 

-   将多行的字符串分组整合成一个字符串，必须配合 **group** 使用

```sql
group_concat([distinct] str1 [order by asc/desc] [separator])
```

**distinct** 可以排除重复值
**order by** 可以按升序 ( **asc** ) 或者降序 ( **desc** ) 进行排序
**separator** 是分隔符，默认为 ','

实例：

```sql
select
    o.class_id,
    group_concat(o.student_name)
from
    student o
group by
    o.class_id
```

上面这个 sql 是将学生按班级进行分组，然后将学生的姓名拼装到一起

更复杂一些的例子，可以将学生的名字、学生的学科和分数进行分组查询并拼接结果

```sql
select
    o.name,
    group_concat(concat_ws('-', o.subject,o.score) order by o.id asc) 
from
    student o
group by o.name;
```

## UNION

UNION 操作符用于连接两个以上的 SELECT 语句的结果到一个结果集合中。多个 SELECT 语句会删除重复的数据。

语法格式：

```sql
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions]
UNION [ALL | DISTINCT]
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions];
```

参数：

-   expression1,expression2,...expression_n: 要查询的列名
-   tables: 要查询的表名
-   WHERE conditions: 可选，查询条件
-   DISTINCT: 可选，删除结果集中重复的数据。默认情况下 UNION 会删除重复数据，所以对结果无影响
-   ALL: 可选，返回所有结果集，包含重复数据
