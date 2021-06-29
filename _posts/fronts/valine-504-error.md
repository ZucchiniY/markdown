---
title: Valine 评论使用报错 504
tags:
  - hexo
  - valine
  - Code 504
categories:
  - 前端技术
  - hexo
description: 解决 valine 报错 504 问题。
abbrlink: c7b31ff
date: 2019-12-19 17:50:20
---

最近准备重新配置一下 [个人博客](https://www.zucchiniy.cn) ，由原来的 [Hugo](https://gohugo.io/) 改到 [Hexo](https://hexo.io/) 来做。

评论系统也由之前的 [disqus](https://disqus.com/) 改成现在的 [valine](https://valine.js.org/) 。

主题也使用了非常好看的 Material Design 的样式的 [Material-x](https://xaoxuu.com/wiki/material-x/) ，并在此之上进行修改。

完成初始的配置之后，做一下测试。

恩。。。报错了？！

![Code 504: The app is archived, please restore in console before use.](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/valine-504-error-1.png)

虽然报错，但是可以正确的显示对应的服务，查询了一下原因，发现是因为长时间未使用 LeanCode 的服务，导致文件上传域名无法访问了，需要在 **设置->应用 Keys** 下面，选择重启 **文件上传域名** 和 **文件访问域名** 的服务即可。

具体的位置如下：

![valine-504-error-2](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/valine-504-error-2.png)
