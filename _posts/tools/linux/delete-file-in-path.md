---
title: 利用 find 命令查找并删除文件
abbrlink: 215ae710
date: 2021-07-04 20:56:09
updated: 2021-07-04 20:56:09
tags:
    - find
    - delete 
categories:
    - 工具环境
    - Linux
keywords:
    - find
    - delete
description: 一条命令删除目录下的文件。
---

## 单纯删除文件

利用 find 命令查找所有的文件并将其删除。

```sh
find . -name '*.DS_Store' -type f -delete
```

## 删除 Git 项目中的内容

删除项目中的所有 **.DS_Store** 。

先从项目中将所有的 **.DS_Store** 查找出来，然后利用命令将文件从 Git 管理中删除。 
```sh
find . -name .DS_Store -print0 | xargs -0 git rm -f --ignore-unmatch
```

然后将 *.DS_Store* 加入到 *.gitignore* 中。

```sh
echo .DS_Store >> ./.gitignore
```

最后再把删除后的版本库提交到远程就可以了。

```sh
git add --all
git commit -m '.DS_Store banished!'
```


