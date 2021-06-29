---
title: .gitignore 文件配置
tags:
  - .gitignore
categories:
  - 工具环境
  - Git
description: 按需求配置 Git 的忽略方案。
type: thematics
abbrlink: 676ce982
date: 2018-05-28 03:18:00
---

git 使用过程中，有许多文件或者文件夹是不希望更新到远程仓库了，因为他们比较占地方，这个时候我们可以利用 `.gitignore` 文件忽略文件。

## 按项目进行忽略 

**.gitignore** 文件用于忽略文件

-   所有空行或者以没注释符号 **#** 开头的行都会被 Git 忽略。
-   可以使用 glob 模式进行匹配。
-   匹配模式最后跟反斜杠 `(/)` 说明忽略的是目录。
-   要忽略指定模式以外的文件或者目录，可以在模式前加上惊叹号。


### glob 模式 

\* : 表示任意个任意字符

? : 表示匹配一个任意字符

所以我们只需要在对应的 **git** 目录下，创建一个 **.gitignore** 文件，然后配置上 **.DS_Store** 即可。

```shell
touch .gitignore
echo */.DS_Store" > .gitignore
```

然后保存，就可以生效了。

## 全局进行配置 

然后我们发现，只要是 Mac 下的 Git 项目我们都需要这样操作一次，太麻烦了，所以我们可以在 home 目录下创建一个 **.gitignore_global** 文件，然后按 **.gitignore** 文件的配置方式完成配置。

在每个项目下的 **.gitignore** 文件中，我们可以引用这个 global 文件。

```shell
git config --global core.excludesfile ~/.gitignore_global
```

这样就可以将全局方法加载到项目配置文件中了。
