---
title: Java 反射获取数据
author: zucchini
tags:
  - reflex
  - Field
categories:
  - 后台技术
  - Java
description: Java 中反射使用的两种方法实现。
abbrlink: 29237c14
date: 2019-03-18 02:50:00
---

## 反射获取成员变量

使用 Sql2o 方法读取数据库的时候，发现表名类似，但是有一些差别，如果使用 `select *` 方式查询，需要针对对象声明多个内容，但是实际上用的都是一样的，所以想通过获取成员变量的名称来拼接成 `select` 后面的内容，经过尝试，发现可以用下面的方法获取：

```java
public String allName(){
    String allName = "";
    Field[] fields = this.getClass().getDeclaredFields();
    for(Field field : fields){
        allName += field.getName() + ",";
    }
    return allName.substring(0, allName.length() -1);
}
```

这样之后，调用 `allName()` 方法就能直接获取对应的变量名称了。

## 反射获取父类实例化对象中的值

```java
try {
    Field[] fields = super.getClass.getDeclaredFields();
    for (Field field : fields) {
        field.setAcessible(true);
        Method method = super.getClass().getDeclaredMethod("get" + upperHeadChar(field.getName()));
        Object obj = method.invoke(super);
        field.set(this, obj);
    }
} catch (NoSuchMethodException | IllegalAcessException | InvocationTargetException e){
    e.printStackTrace();
}

private static String upperHeadChar(String in){
    String head = in.substring(0,1);
    return head.toUpperCase() + in.substring(1);
}
```
