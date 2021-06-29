---
title: git submodule 管理子项目
tags:
  - submodule
categories:
  - 工具环境
  - Git
type: thematics
description: 使用 git submodule 管理子项目模块。
abbrlink: df4b5a39
date: 2018-07-06 08:40:00
---

## 使用场景 

拆分项目，当项目越来越大之后，我们希望 **子模块** 可以单独管理，并由 **专门** 的人去维护，这个时候只可以使用 `git submodule` 去完成。


## 常用命令 

```shell
git clone <repository> --recursive # 递归方式克隆整个项目
git submodule add <repository> path # 添加子模块
git submodule init # 初始化子模块
git submodule update # 更新子模块
git submodule foreach git pull # 拉取所有子模块
```

## 使用方式 


### 添加子模块 

`git submodule add <repository> path` 即可添加

### 克隆子模块 

`git clone <repository> --recursive` 直接递归克隆，如果是克隆父项目，可以在克隆完成之后，使用 `git submodule init` 初始化子项目列表和 `git submodule update` 更新最新的子项目。


### 更新子模块 

如果子模块和新的修改，但是父项目没有更新到最新，则可以使用 `git submodule foreach git pull` 将所有的子项目中更新，如果子项目比 **.gitmodules** 新，则需要更新一下 **.gitmodules** 。

父项目中的子模块的版本是由 **commit id** 标识的，所以需要更新 **.gitmodules** 。


### 删除子模块 

首先需要 `git rm --cached <path>` ，然后依次删除对应的目录、**.gitmodules** 文件中的记录、 **.git/cofig** 中的记录。再提交到远程服务器，就可以删除了。

注意：

> 在执行 `git rm --cached <path>` 的时候，最后不可以有 **/** 。


### 修改子模块配置信息 

与删除相同，需要同时修改 **.gitmodules** 和 **.git/config** 两个文件中的 **URL** 值，然后执行 `git submodule sync` 来同步，然后再提交到远程即可。
