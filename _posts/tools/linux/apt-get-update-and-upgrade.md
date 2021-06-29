---
title: apt-get 中 update 与 upgrade 的区别
tags:
  - apt-get
  - apt
categories:
  - 工具环境
  - Linux
keywords:
  - apt-get
  - apt
  - update
  - upgrade
description: apt-get 源更新，软件包更新。
abbrlink: b38b9bda
date: 2018-08-02 09:14:00
---

`update` : 更新 **/etc/apt/sources.list** 和 **/etc/apt/sources.list.d** 中列出的源的地址,这样才能获取到最新的软件包。
`upgrade` : 升级已安装的所有软件包，升级之后的版本就是本地地址里的，因此，在执行 **upgrade** 之前一定要执行 **update** , 这样才能更新到最新的。
