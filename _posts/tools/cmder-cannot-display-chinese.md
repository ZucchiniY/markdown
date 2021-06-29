---
title: cmder 无法显示中文
author: zucchini
tags:
  - cmder
categories:
  - 工具环境
  - Windows
keywords:
  - cmder
  - set LANG
  - 中文
description: 设置 cmder 支持中文件显示。
abbrlink: d5d74de1
date: 2018-08-29 09:04:00
---


cmder 默认是不支持中文字符的，可以在 **Setting > Startup > Environment** 下增加一行语言设置：

```shell
set LANG=zh_CN.UTF8
```

然后重启 cmder 即可。
