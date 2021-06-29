---
title: zsh shell
tags:
  - zsh
categories:
  - 工具环境
  - Linux
description: 使用 zsh 作为默认的脚本管理工具。
abbrlink: 9d1e89c2
date: 2018-04-29 01:25:00
---

zsh 是一个非常好用的 **shell** ，也是 **bash** 的替代品中比较优秀的一个。


## 启用 

如果未安装，则可以使用对应的命令行进行安装。

`brew install zsh` 或者 `pacman -S zsh` 等方法，然后使用选择器，将默认的 **shell** 设置为 **zsh** 。

```shell
chsh -s `which zsh`
```


## iTerm2 

如果是在 **Mac** 上，可以和 **iTerm2** 一起使用。

## 补全 

zsh 的命令补全功能非常强大，可以补齐路径、命令、参数等。

然后利用 **tab** 键可以在选项中选择，如果过多，可以使用 **ctrl+b** / **ctrl+p** / **ctrl+f** / **ctrl+n** 来进行左上右下的选择。

还有另外一种用法，对于查询到的进程，可以直接转换为 **PID** 进行处理。


## 跳转 


### 省略 `cd` 

zsh 中跳转的时候，可以省略掉 `cd` 这个命令，直接输入 `..` 等同于 `cd ..` 这个命令。

### session 跳转 

在 zsh 中，记录了你最近访问过的地址，可以使用 `d` 命令进行打开，然后按前面序号进行跳转。


## osx 

`cdf` : 在 **Finder** 中打开要 `cd` 的目录

`quick-look` : 快速预览文件，类似在 **Finder** 中按下空格

`man preview` : 在 **preview** 中打开 **man page**

`itunes` : 在命令行中操作 **Itunes**
