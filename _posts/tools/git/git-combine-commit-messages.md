---
title: Git 合并多次提交
tags:
  - rebase
categories:
  - 工具环境
  - Git
type: thematics
keywords:
  - git
  - rebase
  - pick
  - squash
description: 利用 git rebase -i 命令合并多次提交。
abbrlink: d5debf46
date: 2017-08-02 11:33:00
---

在合并分支的时候，希望将多次提交合并成一个，然后再 cherry-pick 到主分支。


## 合并分支 

develop 分支做开发，可能会进行多次提交，但是在发布或者进行 PR 的时候，我们只希望看到一次提交。这个时候，我们需要进行 `git rebase` 之后进行合并。

```shell
# HEAD~3 表示将近三次提交都合并，如果是将 2 次合并则为 HEAD~2
git rebase -i HEAD~3
```

这个时候，看到的是一上对 **COMMIT** 信息的提示

```text
pick 9ba5122 2017 年 8 月 2 日
pick c6da035 ~~

# Rebase 9b6bae1..c6da035 onto 9b6bae1 (2 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

第一列对应的是 `rebase` 具体的操作，其含义如下

| 命令     | 作用                                                           |
|----------|----------------------------------------------------------------|
| pick/p   | git 会应用这个补丁，以同样的提交信息（commit message）保存提交 |
| reword/r | git 会应用这个补丁，但需要重新编辑提交信息                     |
| edit/e   | git 会应用这个补丁，但会因为 amending 而终止                   |
| squash/s | git 会应用这个补丁，但会与之前的提交合并                       |
| fixup/f  | git 会应用这个补丁，但会丢掉提交日志                           |
| exec/x   | git 会在 shell 中运行这个命令                                  |
| drop/d   | git 会移除这次 COMMIT                                          |

将第二个 `pick c6da035 ~~~` 这一行修改成 `squash c6da035 ~~~` ，使之与之前的提交合并。

保存之后可以看到下面的内容

```text
This is a combination of 2 commits.
# This is the 1st commit message:

2017 年 8 月 2 日

删除无用配置，提高启动速度

1. 更新 zucchini-org
2. 增加 CHANGELOG 用来记录每次更新
3. 更新 plantuml 配置
   FIXED Can't find plantuml-jar-path
4. 增加 parinfer 配置，用来优化 lisp 的编写速度

# This is the commit message #2:

~~

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Tue Aug 1 10:24:44 2017 +0800
#
# interactive rebase in progress; onto 9b6bae1
# Last commands done (2 commands done):
"~/spacemacs/spacemacs.d/.git/COMMIT_EDITMSG" 36L, 1003C
```

修改成正确的 `commit` 信息之后，保存存并退出，可以看到下面的内容

```shell
$ git rebase -i HEAD~2
[detached HEAD 0238691] 2017 年 8 月 2 日
 Date: Tue Aug 1 10:24:44 2017 +0800
 5 files changed, 65 insertions(+), 34 deletions(-)
 create mode 100644 CHANGELOG.org
 rewrite local/custom.el (66%)
Successfully rebased and updated refs/heads/develop.
```

这个时候，就已经将我们这几次的更改都合并到一次中了。

## cherry-pick 分支并更新 

这个时候，就可以更新我们的代码了。

首先 `git checkout master` 分支, 然后更新我们的代码 `git pull` 。

然后将我们合并之后的 **develop** 分支的内容更新过来

```shell
git log -b develop
```

看到如下内容

```text
commit 02386914b9e5ab13c23451a3463813bfdecb157a
Author: 语乱 <banshiliuli1990@sina.com>
Date:   Tue Aug 1 10:24:44 2017 +0800

    2017 年 8 月 2 日

    删除无用配置，提高启动速度

    1. 更新 zucchini-org
    2. 增加 CHANGELOG 用来记录每次更新
    3. 更新 plantuml 配置
       FIXED Can't find plantuml-jar-path
    4. 增加 parinfer 配置，用来优化 lisp 的编写速度
```

或者使用上次的操作的中的提示 `[detached HEAD 0238691] 2017 年 8 月 2 日` 其中的 **0238691** 就是我们需要

```shell
git cherry-pick 0238691
```

这样我们再推送到远程就可以实现合并更新了。
