---
title: Git 基础命令
tags:
  - Git
  - command
categories:
  - 工具环境
  - Git
keywords:
  - git
  - command
  - basis
description: Git 基础命令整理。
group: notes
abbrlink: c5f0918d
date: 2016-04-27 07:02:00
---

## 新建代码库 

```shell
#在当前目录新建一个 git 代码库
$ git init
#新建一个目录，将其初始化为 git 代码库
$ git init [project-name]
#下载一个项目和它的整个代码历史
$ git clone [url]
```



## 全局配置和项目配置 

git 的设置文件为 `.gitconfig` ，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）

```shell
#显示当前 git 配置
$ git config --list
#编辑 git 配置文件
$ git config -e [--global]
#设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```


## 增加/删除文件 

```shell
#添加指定文件到暂存区
$ git add [file1] [file2] ...
#添加指定目录到暂存区，包括子目录
$ git add [dir]
#添加当前目录的所有文件到暂存区
$ git add .
#删除工作区文件，并且将这次删除放入到暂存区
$ git rm [file1] [file2] ...
#停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]
#改名文件，并将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

## 代码提交 

```shell
#提交暂存区到仓库区
$ git commit -m [message]
#提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]
#提交工作区自上次 commit 之后的变化，直接到仓库区
$ git commit -a
#提交时显示所有 diff 信息
$ git commit -v
#使用一次新的 commit,替代上一次提交
#如果代码没有变化，则用来改写上一次的 commit 的提交信息
$ git commit --amend -m [message]
#重做上一次 commit, 并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
```

## 分支 

```shell
#列出所有本地分支
$ git branch
#列出所有远程分支
$ git branch -r
#列出所有本地分支和远程分支
$ git branch -a
#新建一个分支，但依然停留在当前分支
$ git branch [branch-name]
# 将原有分支名称 old branch name 修改为 new name
git branch -m <old branch name> <new name>
# 修改当前分支名称为 new name
git branch -m <new name>
#新建一个分支，并切换到当该分支
$ git checkout -b [branch-name]
#新建一个分支，指向指定 commit
$ git branch [branch] [commit]
#新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]
#切换到指定分支，并更新工作区
$ git checkout [branch-name]
#建立追踪关系，在现有分支与指定的远程分支之前
$ git branch --set-upstream [branch] [remote-branch]
#合并指定分支到当前分支
$ git merge [branch]
#选择一个 commit,合并进当前分支
$ git cherry-pick [commit]
#删除分支
$ git branch -d [branch-name]
#删除远程分支
$ git push origin --delete [branch-name]
#删除与远程分支关联
$ git branch -dr [remote/branch]
#删除远程分支 2
$ git branch -r -d origin/[branch-name]
$ git push origin :[branch-name]
```


## 标签 

```shell
#列出所有 tag
$ git tag
#在当前 commit，新建一个 tag
$ git tag [tag]
#在指定 commit，新建一个 tag
$ git tag [tag] [commit]
#删除本地 tag
$ git tag -d [tag]
#删除远程 tag
$ git push origin :refs/tags/[tagName]
#查看 tag 信息
$ git show [tag]
#提交指定的 tag
$ git push [remote] [tag]
#提交所有 tag
$ git push [remote] --tags
#新建一个分支，指向某个 tag
$ git checkout -b [branch] [tag]
#重命名 tag
$ git tag -f [new-tagName] [old-tagName]
$ git tag -d [old-tagName]
#将本地 tag 推送到远程
$ git push origin :refs/tags/[old-tagName]
$ git push --tags
# 拉取 tag
git fatch origin tag tag_name
```


## 查看信息 

```shell
#显示所有变更的文件
$ git status
#显示当前分支的版本历史
$ git log
#显示 commit 历史，以及每次 commit 发生变更的文件
$ git log --stat
#显示某个 commit 之后的所有变动，每个 commit 占据一行
$ git log [tag] HEAD --pretty=format:%s
#显示某个 commit 之后的所有变动，其“提交说明”必须符合条件
$ git log [tag] HEAD --grep feature
#显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]
#显示指定文件相关的每一次 diff
$ git log -p [file]
#显示指定文件是什么人在什么时间修改过
$ git blame [file]
#显示暂存区与工作区的差异
$ git diff
#显示暂存区和上一个 commit 的差异
$ git diff --cached [file]
#显示工作区与当前分支最新 commit 之间的差异
$ git diff HEAD
#显示两次提交之间的差异
$ git diff [first-branch] ... [second-branch]
#显示某次提交的元数据和内容变化
$ git show [commit]
#显示某次提交发生的变化的文件
$ git show --name-only [commit]
#显示某次提交时，某个文件的内容
$ git show [commit]:[filename]
#显示当前分支的最近几次提交
$ git reflog
    ```



## 远程同步 

```shell
#下载运程仓库的所有变动
$ git fetch [remote]
#显示所有远程分支
$ git remote -v
#显示某个远程仓库的信息
$ git remote show [remote]
#增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]
#取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]
#上传本地指定分支到远程仓库
$ git push [remote] [branch]
#强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force
#推送所有分支到远程仓库
$ git push [remote] --all
```



## 修改远程仓库地址 

```shell
# 先删除远程分支地址
$ git remote rm origin
# 然后重新增加远程分支地址
$ git remote add origin [url]
```



## 撤销 

```shell
#恢复暂存区的指定文件到工作区
$ git checkout [file]
#恢复某个 commit 的指定文件到工作区
$ git checkout [commit] [file]
#恢复上一个 commit 的所有文件到工作区
$ git checkout .
#重置暂存区的指定文件，与上一次 commit 保持一致，但工作区不变
$ git reset [file]
#重置暂存区与工作区，与上一次 commit 保持一致
$ git reset --hard
#重置当前分支的指针为指定 commit，同时重置暂存区，但工作区不变
$ git reset [commit]
#重置当前分支的 HEAD 为指定 commit，同时重置暂存区和工作区，与指定 commit 一致
$ git reset --hard [commit]
#重置当前 HEAD 为指定 commit，但保持暂存区和工作区不变
$ git reset --keep [commit]
#新建一个 commit, 用来撤销指定 commit
#后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]
```



## 其它 

```shell
#生成一个可供发布的压缩包
$ git archive
```


## git 提升内容 

-   储藏暂存内容

```shell
# 想要切换分支，但是还不想要提交之前的工作，可以储存修改信息，将新的储藏推送到栈上
$ git stash / git stash save
# 在这时，能够轻易的切换分支并在其他地方工作，你的修改被存储在栈上。要查看储藏的东西，可以使用 git stash list
$ git stash list
# 可以将刚刚的储藏重新加载回来
$ git stash apply
# 也可以通过储藏的序号进行加载
$ git stash apply stash@{1}
```

-   核武器级选项 filter-branch

```sh
# 从每一个提交移除一个文件：指 git add . 的内容完整的上传到仓库，但是当希望开源这个内容的时候，需要移除一些无用的文件，--tre-filter 选项在的每一个提交后，运行指定的命令，然后重新提交结果。
$ git filter-branch --tree-filter 'rm -f passwords.txt' HEAD
# 使一个子目录作为新的根目录：假设已经从另一个源代码控制系统中导入，并且有几个没意义的子目录（trunk/tags 等等）。如果想要让 trunk 子目录作为每一个提交的新的项目根目录，filter-branch 也可以帮助你那么做，再在新项目根目录是 trunk 子目录且 Git 会自动移除所有不影响子目录的提交。
$ git filter-branch --subdirectory-filter trunk HEAD
# 在开始工作时忘记运行 git config 来设置你的名字与邮箱地址，或者你想要开源一个项目，并且修改所有你的工作邮箱地址为你的个人邮箱地址。任何情形下，你也可以通过 filter-branch 来一次性修改多个提交中的邮箱地址。需要小心的是只修改你自己的邮箱地址，所以使用 --commit-filter 来修改：
$ git filter-branch --commit-filter '
    if [ "$GIT_AUTHOR_EMAIL" = "schacon@localhost" ];
    then
        GIT_AUTHOR_NAME = "scott Chacon";
        GIT_AUTHOR_EMAIL = "schacon@example.com";
        git commit-tree "$@";
    else
        git commit-tree "$@";
    fi' HEAD
```

