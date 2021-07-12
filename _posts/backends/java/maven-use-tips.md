---
title: Maven 使用笔记
author: zucchini
tags:
  - Maven
categories:
  - 后台技术
  - Java
description: Maven 入门命令与配置。
abbrlink: 2200c068
date: 2018-04-18 07:06:00
group: notes
---

## 创建一个项目 

```shell
mvn archetype:generate -DarchetypeCatalog=internal
-DgroupId=com.mycompany.app -DartifactId=my-app
-DarchetypeArtifactId=maven-archetype-quickstart
-DinteractiveMode=false
```

-   `mvn archetype:generate` 固定格式
-   `-DgroupId` 组织标识，包名
-   `-DartifactId` 项目名称
-   `-DarchetypeCatalog=internal` 不要从远程服务器上取 catalog，解决新建项目卡在 Generating project in interactive mode 处的问题
-   `-DarchetypeArtifactId` 指定 ArchetypeId ,
    `maven-archetype-quickstart` , 创建一个 java 项目；
    `maven-archetype-webapp` ，创建一个 web 项目
-   `-DinteractiveMode` 是否使用交互模式


## 修改本地仓库路径 

在 `setting.xml` 中增加下面的配置，将 **本地地址** 改成对应的路径即可。

```xml
<localRepository>本地地址</localRepository>
```

## 导出工程依赖的 jar 包 

-   导出到默认目录下

```shell
mvn dependency:copy-dependencies
```

-   导出到指定目录下

```shell
mvn dependency:copy-dependencied -DoutputDirecrtory=lib
```

-   设置依赖级别，并导出到对应的目录下

```shell
mvn dependency:copy-dependencied -DoutputDirecrtory=lib -DincludeScope=jcompile
```

对应的5个级别：

-   complie: 表示 dependency 都在生命周期中使用，同时会传递到依赖项目中
-   provided: 表示 dependency 由 JDK 或者容器提供，只作用在编译和测试时，无传递性
-   runtime: 表示 dependency 不作用在编译时，但会作用在运行和测试时
-   test: 表示 dependency 作用在测试时，不作用在运行时，不随项目发布
-   system: 与 provided 类似，但是在系统中要以外部 jar 包形式提供，maven 不会在 repository 查找它

## 使用华为镜像 

在 `setting.xml` 文件中 **mirrors** 节点中添加下面的内容：

```xml
<mirror>
  <id>huaweicloud</id>
  <mirrorOf>*</mirrorOf>
  <url>https://mirrors.huaweicloud.com/repository/maven/</url>
</mirror>


另外华为的镜像站为 [https://mirrors.huaweicloud.com](https://mirrors.huaweicloud.com/)。

## maven 常用命令 

| 命令                | 作用                                                   |
|---------------------|--------------------------------------------------------|
| mvn clean           | 清理项目生产的临时文件，一般是模块下的 target 目录     |
| mvn compile         | 编译源代码，一般编译模块下的src/main/java目录          |
| mvn package         | 项目打包工具,会在模块下的target目录生成jar或war等文件  |
| mvn install         | 将打包的jar/war文件复制到你的本地仓库中,供其他模块使用 |
| mvn deploy          | 将打包的文件发布到远程参考,提供其他人员进行下载依赖    |
| mvn site            | 生成项目相关信息的网站                                 |
| mvn dependency:tree | 打印出项目的整个依赖树                                 |
| mvn spring-boot:run | 启动 springboot 项目                                   |
