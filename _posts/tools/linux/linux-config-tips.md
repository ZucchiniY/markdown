---
title: Linux 的环境配置
tags:
  - pacman
  - plantuml
categories:
  - 工具环境
  - Linux
description:
  - 调整 Manjaro 的源为国内并刷新源
  - 解决 PlantUML 中文乱码问题
abbrlink: 291b15f4
date: 2017-06-08 08:49:00
---

## 切换更新源

刷新 **Manjaro** 源，由快到慢并指定为中国源

    :::shell
    sudo pacman-mirrors -gb testing -c China

然后更新系统：

    :::shell
    sudo pacman -Syyu

## plantuml 中文乱码

在 Linux 系统中，无论是官方 **JDK** 还是 **OpenJDK** 都有中文字库不全的问题。需要通过安装默认字体来解决这个问题：

    :::shell
    sudo dnf install cjkuni-uming-fonts

安装之后，重新执行 **PlantUML** 代码块，中文可以正常显示。
