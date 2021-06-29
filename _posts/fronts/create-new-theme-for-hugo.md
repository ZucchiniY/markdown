---
title: 编写自己的 Hugo 主题
tags:
  - Hugo
  - theme
categories:
  - 前端技术
  - hugo
keywords:
  - hugo theme
  - create theme
description: 编写自己的 Hugo 主题。目前该主题不再更新，博客也会逐步迁移到 Hexo 上。
abbrlink: c6e7960e
date: 2018-08-22 06:38:00
---

## 页面 

使用 `hugo new theme paladin` 直接创建一个新的主题，然后可以在当前博客中（已经完成了多篇文章，但是还想自己定义一个主题）或者在新主题中增加测试用的项目。

目前所实现的大概样式如下：

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/sn01.png)

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/sn02.png)

创建之后，在 **themes** 目录下可以看到整个项目结构:

    themes/paladin
    ├── LICENSE
    ├── archetypes
    │   └── default.md
    ├── layouts
    │   ├── 404.html
    │   ├── _default
    │   │   ├── baseof.html
    │   │   ├── list.html
    │   │   └── single.html
    │   ├── index.html
    │   └── partials
    │       ├── footer.html
    │       └── header.html
    ├── readme.md
    ├── static
    │   └── css
    │       └── stylesheet.css
    └── theme.toml
    6 directories, 12 files

可以看到目录下有一些已经创建好的 **html** 目录，有几个需要编辑的，分别是 **single.html** ， **index.html** ， **404.html** ， **footer.html** 和 **header.html** 这几个文件。


## _default 

这里放的，是主要几个网站模板，用来提交一些默认的配置的。

- single.html

这个是用来渲染生成的单页文章的，主要是 **content/** 下的内容，可以用来渲染页面的名称、作者、时间和文章的具体内容。

- list.html

这个是用来渲染生成的列表页的，包括文章列表页或者是标签列表和分类列表页。

### partials 

这个目录下主要是放需要利用的代码片断，通过 **partial** 方法调用。

- header.html

这里主要定义 `<head>` 标签和导航栏 `<nav>` 相关内容。

- footer.html

这里定义了网页脚标位置的相关内容。


## 实现 

主题参照了 [hugo-theme-hiruko](https://github.com/GenkunAbe/hugo-theme-hiruko) 的样式，去掉了一些用不到的功能。

主要使用了[bootstrap4](https://getbootstrap.com)，其中的一些图标来源自[阿里巴巴的矢量库](http://www.iconfont.cn)，用起来方便快捷。

当文章过多时，可以使用连续页面的样式，如果不想使用，可以用上一页下一页的方式。通过参数 **paginateOriginalStyle** 来控制，如果为 **true** 则是上一页下一页的样子，如果是 **false** 则如下图：

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/sn03.png)

将社交链接和logo放到到 **about.html** 页面中，可以方便的看到作者的相关内容。

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/sn04.png)

如果想修改logo的话，需要修改主题目录下的 **static/media/zlogo.png** 文件即可。
