---
title: GitHub 做为博客图床
tags:
  - 图床
  - jsdelivr
  - github
categories:
  - 工具环境
  - GitHub
description: 上传图片到 GitHub ，然后通过 jsDelivr 访问。
abbrlink: de549f00
date: 2020-01-15 10:48:57
---

博客上传到 GitHub 之后，访问文章中的图片会有一些慢，如果是比较大的图片就更慢了。

之前是通过使用的一些公共的图床网站解决的，但是会有大小和数量的限制，最近看到一个图床应用[jsDelivr](https://www.jsdelivr.com/)，这个可以直接访问 GitHub 里的图片信息，所以调整了一下原本的图床方案。

使用也非常容易：

在 GitHub 上新建一个版本库，然后将图片放到项目中，上传到对应的分支，比如 master 分支，我的示例是在 [blog-assets](https://github.com/ZucchiniY/blog-assets) 项目，比如图片放在 _avatar_ 路径下，文件名叫 _lvzai.jpg_——也就是我博客的用户头像，然后将相关内容上传到 GitHub 上之后，就可以通过 [https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/avatar/lvzai.jpg](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/avatar/lvzai.jpg) 查看图片了。

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/avatar/lvzai.jpg)
