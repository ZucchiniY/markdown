---
title: Mac 电脑上使用 Emacs
tags:
  - install Emacs
  - Mac OS
categories:
  - 工具环境
  - Emacs
keywords:
  - brew
  - railwaycat/emacsmacport
  - emacsformacosx
description: 通过 brew 安装 Emacs
abbrlink: d9063434
date: 2019-07-29 03:17:00
---

在 Mac 上使用 Emacs 有两个方案，从 [Emacs For Mac OS X](https://emacsformacosx.com/) 手工下载，然后更新本地，或者是在 **homebrew** 中增加配置，然后利用 `brew upgrade` 从 [homebrw-emacsmacport](https://github.com/railwaycat/homebrew-emacsmacport) 上进行下载和更新。

两种方式获取的 Emacs 有少许不同，具体的见两个项目的简介。

第二种方法的命令如下：

```shell
brew tap railwaycat/emacsmacport
brew install emacs-mac
```

安装之后，如果要从启动台启动应用，需要在 */applications* 和安装位置增加软链接，命令如下

```shell
ln -s /usr/local/opt/emacs-mac/Emacs.app/Applications
```

这样之后就可以直接在 **Alfred** 中输入 `emacs` 直接启动。
