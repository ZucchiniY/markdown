---
title: in 和 exists 的不同
tags:
  - exists
  - in
categories:
  - 后台技术
  - MySQL
description: SQL 中使用 in 和 exists 时的考虑。
abbrlink: 4a975a81
author: zucchini
date: 2018-07-26 09:23:00
---

## in OR exists 

in 是把外表和内表做 **hash** 连接，而 exists 是对外表作 **loop** 循环，每次 **loop** 循环再对内表进行查询。

简单的认为 exists 比 in 的效率高的说法是不准确的。

如果两个表大小相当，则 in 和 exists 的效率是差不多的，如果两个表的一大一小，则子查询表大的用 exists，子查询表小的用 in。

```sql
select * from a where id in(select id where b);
```


即我们可以理解， in 实际上是做了两个循环：

```c
for(int i=0;i< a.length;i++){
    for(int j = 0; j < b.length;j++){
        if(a[i].id == b[j].id){
            return a[i];
        }
    }
 }
```

所以极限点是 **a.length * b.length** 。

同理，可以把 exists 理解为：

```c
for(int i = 0;i < a.length;i++){
    if(exists(a[i].id)){
        return a[i];
    }
 }
```

这里需要说明的是： `exists(a[i].id)` 的过程，实际上是去数据库中查询 b 表的过程。

所以在看这两种查询的时候，如果 a 表有10000条记录，b表有100条记录，则 in 需要遍历 10000 * 100 次，但是如果 b 表有 10000000 条记录，则 in 需要 10000 * 10000000 次。同样的数据，如果是使用 exists 的话，则是需要做一个 10000 次数据库查询，所以 **子查询的表较大时，最好使用 exits 去做查询**。但是如果两个表差不多大，或者子查询的表较小的时候，就可以选择 in 做查询了。

## not in OR not exists 

not in 和 not exists 两个的选择就比较简单了，就是仅使用 not exists 即可。其原因主要有两个：

### not in 会出现 BUG 

表t1

| c1 | c2 |
|----|----|
| 1  | 2  |
| 1  | 3  |

表t2

| c1 | c2 |
|----|----|
| 1  | 2  |
| 1  |    |

先执行 `not in`

```sql
select * from t1 where t2 not in(select c2 from t2);
```

这个时候，我们可以看到，先查询出 t2.c2 的值(2,null), 也就是，我们把这个语句变成了 `select * from t1 where t2 <> 2 and t2 <> null` 。

这是为什么呢？

这主要是因为 null 是无法进行 /操作/ 的，也就是 null 的几个原则：

1.  如果 null 参与算术运算，则该算术表达式的值为 null 。
2.  如果 null 参与比较运算，则结果可视为 false 。
3.  如果 null 参与聚集运算，则聚集函数都置为 null 。除 count(*) 之外。

这个时候，我们可以看到，查询回来的结果是空，但是这并不是我们想看到的。这时我们来测试一下 `not exists` 方法。

```sql
select * from t1 where not exists(select c2 from t2 where t2.c2 = t1.c2);
```

得到的结果是

| c1 | c2 |
|----|----|
| 1  | 3  |

OK，这就是我们想要的结果。

### not in 比 not exists 慢 

如果查询语句使用了 `not in` 那么内外表都进行全表扫描，没有用到索引；而 `not extsts` 的子查询依然能用到表上的索引。所以无论那个表大，用 `not exists` 都比 `not in` 要快。

所以，我们在选择的时候，不要使用 `not in` 而是需要将这些语句用 `not exists` 来替换。
