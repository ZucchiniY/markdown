---
title: Hexo 引用自己撰写的文章
tags:
  - post link
  - post path
categories:
  - 工具环境
  - Hexo
description: 在 Hexo 博客中，可以通过文件的形式自动生成引用链接，还不必直接输入对应的链接名。
abbrlink: bd7386da
date: 2020-01-15 14:20:52
---

文章中，有时候需要自己给自己引流，所以经常要引用自己的文章，原来在使用 Hugo 的时候，是用的直接写入文章最终链接的方法引用，虽然可以成功的引用文章，但是如果原本的文章链接变化了，就不能使用了，所以最好的方法就是在生成系统之内直接引用。

Hexo 提供了 [标签插件](https://hexo.io/zh-cn/docs/tag-plugins.html) 来完成这个功能。

    :::markdown
    {% post_path filename %}
    {% post_link filename [title] [escape] %}

比如想要引用我的某一篇文章，需要写 `post_link '工具环境/github-jsDelivr'` 就可以在文章中看到：{% post_link '工具环境/github-jsDelivr' %}，这样就可以进行站内文章的引用了，这里展示的是文章中的 title 字段，而不是文件名，但是要注意的是，这里默认的路径是在 _post 路径下，如果不是默认路径，需要写上相对路径。

当然，也可以按自己的想法，定义一个名称，比如 `post_link '工具环境/github-jsDelivr' '测试'` 这样，我们看到的链接是有个人定义的名称的：{% post_link '工具环境/github-jsDelivr' '测试' %}。这两个展示的名称不同，但是最终指向的都是同一篇文章。

另外还有一种方案，是使用 `post_path` 指定文章的链接地址，但是不是链接，比如 `post_path '工具环境/github-jsDelivr'`，我们在文章中看到的是: {% post_path '工具环境/github-jsDelivr' %} ，主要可以直接插入文件链接，也很方便。

相比较而言，`post_link` 的方式更常用，但是有些时候，使用 `post_path` 也可以帮助我们获取信息。
