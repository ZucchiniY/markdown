---
title: Mac 上执行命令报错解决方案
tags:
  - xcode
  - xcrun
  - node-gyp
categories:
  - 工具环境
  - Mac
description: '解决 invalid active developer path, command line tools are already installed 报错。'
abbrlink: 8362cb1a
date: 2017-12-07 07:31:00
updated: 2020-01-14 00:00:00
---

## git 

mac 执行 git 命令时候出现 `invalid active developer path` ：

具体如下：

```shell
xcrun: error: invalid active developer path
(/Library/Developer/CommandLineTools), missing xcrun at:
/Library/Developer/CommandLineTools/usr/bin/xcrun
```

解决方法：

打开终端输入

```shell
xcode-select --install
```

回车后，系统弹出下载 xcode，点击确认，下载完成后即可。（实际上不是下载 xcode，可能下载 xcode 有关插件，下载时长约 1 分钟）

原因 : **出现这个错误原因猜想可能是因为之前安装过 xcode 卸载后或者是因为 xcode 更失丢失内容导致的。**

## node-gyp 

安装 node-gyp 的时候报错 **xcode-select: error: command line tools are already installed, use "Software Update" to install updates** 。

解决办法是重新安装 CommandLineTools 工具，用下面的命令处理：

```shell
softwareupdate --install -a # 查看安装状态
softwareupdate --list # 查看安装的列表
reinstall xcode-select # 重装 xcode-select
sudo rm -rf /Library/Developer/CommandLineTools # 删除旧的版本
xcode-select --install # 自动重新安装
```
