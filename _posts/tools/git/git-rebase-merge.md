---
title: Git 解决分支冲突
tags:
  - rebase
  - merge
categories:
  - 工具环境
  - Git
type: thematics
description: 利用新建分支或者暂存修改的方法解决提交冲突。
abbrlink: a7a0f436
date: 2017-05-07 09:25:00
---

在使用 git 进行版本管理的开发过程中，经常遇到上传或者拉取分支的时候冲突，在遇到冲突的时候，经常使用下面两个方式解决，虽然第一个方案看起来比较复杂，但是如果按我之前的文章: {% post_link '工具环境/git/git-workflow' %} 进行工作的话，只需要执行3、4、5三步即可。

虽然提供的解决方案，但是最好还是从根源上降低冲突出现的频率才是最好的方案。

## 新建分支方法

本文主要讨论 Git feature 与 **master(develop)** 分支冲突解决方案。

1. `git pull` : 同步远程分支，发现当前的开发流有了新的提交，且与自己开发的功能有冲突。
2. `git checkout -b feature` : Checkout 到 feature 分支。
3. `git checkout master` `git pull origin master` : 切换到 master 分支并拉取最新的内容。
4. `git checkout feature` `git rebase master`: 切换到 feature 分支并将 master 的修改合并，并解决冲突。
5. `git add -A` `git rebase --continue` : 将修改内容保存并继续 rebase 操作。
6. `applying: xxxx` : 看到这个提示表示已经完成了合并。
7. `git checkout master` `git merge feature` : 切换到 master 分支并将 feature 分支内容合并过来。

## 暂存提交方案

在修改的时候，忘记新建对应的分支了，可以按上面的方案，但保存，然后创建新的分支，再将远程分支对应分支的内容 `reset` 回未修改的状态。或者使用 `git stash` 系列命令解决冲突。

1. `git stash` : 暂存修改的内容
2. `git pull` : 拉取最新的内容
3. `git stash apply` or `git stash pop` : 将暂存的内容合并进来

## git stash 命令

`git stash apply` : 应用暂存内容但是不删除，可以是最近的一次暂存，也可以按序号应用 `git stash apply stash@{0}`
`git stash drop` : 移除暂存的内容
`git stash pop` : 应用的同时从列表中移除，只能操作最近的一次 stash 的内容
`git stash list` : 查看整个的暂存列表
`git stash save` : 来查看对应的所有的修改，这样就可以非常方便的找到最好的实现方案
`git stash show -p stash@{1}` : 不输入对应的 `stash@{}` 内容则将最近的 stash 与当前分支做比较，如果加了则用指定的暂存

- Git stash apply 的时候，报错 :

```text
error Your local changes to the follow files would be overwritten by merge: xxxx
Please commit your changes or stash them before you merge .
```

可以先add 修改的文件，然后再apply

```shell
git add test.txt
git stash apply
```
