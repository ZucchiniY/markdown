---
title: GitHub Pull Request 方案
tags:
  - GitHub Pull Request
  - pr
categories:
  - 工具环境
  - Git
type: thematics
description: 利用 PR 向 GitHub 的开源项目贡献代码。
abbrlink: bf0a41db
date: 2017-08-07 03:08:00
---

## 操作流程 

- 登录自己的账号，然后克隆一下原始项目。
- 将自己账号下的项目克隆到本地。
- 为了追踪原始仓库的更新，需要添加要更新的分支的原始仓库为远程分支

```shell
git remote add upstream <origin>/<xxxx>
```

-   创建私有分支 _develop_ ，用来开发项目

```shell
git checkout -b develop
```

- 本地 _develop_ 分支提交
- 切换 _master_ 分支，同步原始仓库

```shell
git checkout master
git pull upstream master
```

-   切换本地 _develop_ 分支，合并本地 _master_ 分支并解决冲突
-   提交本地 _develop_ 分支到自己的 _develop_ 分支
-   向原始仓库发起 **Pull Request** 请求
-   等待原作者回复 (接受/拒绝)


## 注意点 

> 在拉取新分支时，最好使用 **rebase** ，需如果使用 **merge** 的话，会增加许多 **commit** 信息，这会降低更新的整洁性。

如果有许多提交，可以先将自己的修改合并的一起，再进行提交，合并方案可以参考 {% post_link '工具环境/git/git-combine-commit-messages' %}。

如果在提交的时候与远程有冲突，或者希望在本地解决冲突问题，可以参考 {% post_link '工具环境/git/git-rebase-merge' %}进行冲突解决。
