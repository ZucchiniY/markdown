---
title: Git 补遗
tags:
  - cache
  - reset
categories:
  - 工具环境
  - Git
type: thematics
description: 一些不常用但很实用的方案，在需要的时候方便使用。
abbrlink: afae679a
date: 2018-04-20 11:22:00
---

## 文件退出暂存区，但是保留修改 

在代码或者一些内容更新完成好，进行了 `git add .` 或者 `git add -A` 操作，但是发现操作错误了，不希望进行暂存区，但是又不想移除已经修改的内容，可以执行 `git reset --mixed` 操作，这样将文件退出暂存区，但是修改的内容保留。


## 多次修改，一次 commit 

在进行一个功能的开发过程中，希望将整个功能仅做一次 _commit_ ，可以在修改完成后，执行 `git add .` ， 然后再执行 `git commit --amend` ，这样可以把修改的内容分次写入到 _commit_ 文件中，最后再进行提交。


## git 移除 cache 的内容 

-   git 删除暂存区的文件，不会移除文件，即保留工作区。

```shell
git rm --cache fileName
#fileName 为对应的文件名
```

-   删除暂存区和工作区的文件

```shell
git rm -f fileName
```

## git 删除错误的 commit 

**commitId 为对应的 id**

- 仅仅撤销已经提交的版本库，不会个性暂存区和工作区

```shell
git reset --soft commitId
```

- 撤销已提交的版本库和暂存区，不会修改工作区

```shell
git reset --mixed commitId
```

- 彻底将工作区、暂存区和版本库记录恢复到指定的版本

```shell
git reset --hard commitId
```

> 如果你希望保留修改，但是撤销提交，则使用 `--mixed` ，如果想彻底恢复，则使用 `--hard`
