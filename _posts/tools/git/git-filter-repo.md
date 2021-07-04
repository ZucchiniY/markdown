---
title: 清理 Git 项目的大小
tags:
  - git
  - git-filter-repo
categories:
  - 工具环境
  - Git
keywords:
  - Git
  - filter-repo
description: 通过删除 git 项目中不再跟踪的文件记录来缩减 git 项目的大小。
abbrlink: 5ea56af6
date: 2021-07-03 21:04:41
updated: 2021-07-03 21:04:41
---

## 起因

一直使用 Emacs 作为自己的编辑器，在单位电脑进行下载配置的时候发现项目的大小竟然已经到了 67M ，在下载过程中太长了，就看了一下目录，主要是因为之前在使用 lsp-mode 和 rime 输入法的时候，项目中引入了许多二进制文件，而在 Git 的管理中，二进制文件是无法跟踪其变化的，所以在后面移除了相关的配置，但二进制文件还在项目中，就想在整个项目之中删除相关的文件和文件夹。

## 方法

之前整理过 Git 相关的命令，其中推荐的方法是使用 filter-branch 命令进行删除，在使用命令中发现提示我使用 git-filter-repo 来修改，就查看了一下命令，使用了一下，感觉很简单，现在把处理过程记录下来。

### 安装

命令是 Python 实现的，所以安装的时候也需要在环境上配置过 Python 环境，安装命令 `pip install filter-repo` 。

> 如果是 Mac 的话，可以通过 `brew install git-filter-repo` 来安装。

### 使用

可以利用 filter-repo 命令分析整个项目中的文件，找到已经删除掉的文件是项目中比较大的文件。

``` sh
git filter-repo --analyze
```

然后再利用生成的文件，可以通过另一个命令清理掉对应的文件。

```sh
git filter-repo --invert-paths --paths-from-file file-want-to-delete.txt
```

一般我是利用在 `.git/filter-repo/analysis/` 目录下 `path-all-sizes.txt` 文件和 `path-deleted-sizes.txt` 两个文件，找到希望不再跟踪的文件，另存到一个文件 **file-want-to-delete.txt** 中。

然后通过第二个命令，将需要删除的文件可以清理掉，然后再强制推到远程就可以了。

### 建议

对于一些多人合作的项目，不建议进行清理的，因为有一些文件会登录有具体的项目流程等内容，所以尽量不要清理文本文件，而二进制文件，其实最好在一开始就不要纳入到 Git 进行管理，而且作为单独的文件管理就比较好。
