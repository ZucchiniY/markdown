---
title: Git 上传文件超出最大值
tags:
  - postbuffer
  - hung up
categories:
  - 工具环境
  - Git
type: thematics
description: 如何解决 git hung up unexpectedly error。
abbrlink: a2153400
date: 2018-08-30 09:00:00
---

昨天在上传打包的程序的时候，git 意外报错了，试了一下，并不是因为有冲突什么的，仔细看了一下报错：

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/git-rpc-error.png)

发现是 OpenSSL 中报的错，确认了一下 error 发现是因为上传的文件过大导致的。需要将 **postbuffer** 调整一下：

```shell
git config http.postbuffer 523288000
```

再上传一次，果然可以了。查询一下看看到底修改了什么

```shell
git config --list
```


再查看一下：

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/git-http.png)

确认是修改了对应的最大 **post** 的请求的值。
