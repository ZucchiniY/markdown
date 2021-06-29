---
title: SpringBoot 常用配置
tags:
  - mysql
  - oracle
  - 模糊查询
  - logging level
categories:
  - 后台技术
  - Java
description: 在 MyBatis 中利用 SQL 进行模糊查询。
abbrlink: d3f4d776
author: zucchini
date: 2018-04-28 07:14:00
---

## MyBatis 模糊查询

### mysql 

```sql
SELECT * FROM DB.SQL WHERE MYNAME LIKE CONCAT('%' , #{myName} , '%')
```

或者使用 **${}** 来进行查询

```sql
SELECT * FROM DB.SQL WHERE MYNAME LIKE CONCAT('%' , '${myName}' , '%')
```


### Oracle 

```sql
SELECT * FROM DB.SQL WHERE MYNAME LIKE '%'||#{myName}||'%'
```

## 拆分 SpringBoot 的基础 lib 包

最近发现使用 Springboot 项目上传到服务器越来越慢，所以决定将项目拆分一下，将需要的 _lib_ 包拆分开来。

首先需要按原来的内容进行打包，然后就打好的包解压，然后将 **BOOT-INF** 下的内容，上传到服务器，然后将 _pom.xml_ 文件中的 **org.springframework.boot** 增加 **configuration** 的配置，增加之后如下：

```xml
<plugin>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-maven-plugin</artifactId>
  <configuration>
    <layout>ZIP</layout>
    <includes>
      <include>
        <groupId>nothing</groupId>
        <artifactId>nothing</artifactId>
      </include>
    </includes>
  </configuration>
</plugin>
```

然后再重新打包，生成的内容就只有自己编写的内容了。

上传到服务器之后，先新建一个 **shell** 角本，然后增加执行权限： `chmod x+a start.sh` 然后将启动的脚本增加如果下内容：

```shell
java -Dloader.path=./lib -jar ./xxx.jar
```

再启动的时候，更新了代码，打包再上传服务器，也就一分钟的事儿。

## Springboot 日志级别

-   打印 Mybatis 中调用的 Sql

```yml
logging:
    level:
        com.xxxx.mapper: DEBUG
```

**com.xxxx.mapper 是 Mybatis 接口的影射文件包**

-   Logger 日志按级别打印

```yml
logging:
    level:
        org.springframework.web: DEBUG
```

其中，日志级别有： **ERROR** **WARN** **INFO** **DEBUG** **TRACE**

-   root 的日志级别设置

```yml
logging:
    level:
        root: DEBUG
```
