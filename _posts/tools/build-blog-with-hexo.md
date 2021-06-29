---
title: 搭建个人博客
author: zucchini
tags:
  - hexo
  - hugo
  - GitHub Pages
categories:
  - 工具环境
  - hexo
keywords:
  - hexo
  - hugo
  - ox-hugo
  - github pages
description: 利用 hugo 或者 hexo 生成静态博客并保存到 GitHub Pages 上。
abbrlink: c442673f
date: 2020-01-05 00:00:00
---


**我们无法选择生活的样子，但我们可以记下来。**

## 博客的开始 

其实，一切都是为了更好的装逼。好吧，我着相了。

最开始想做一个自己博客，主要是因为看到了很多人都有，觉得自己没有太 Low 了。于是申请了 CSDN 的用户，但是太丑了，于是又申请了博客园，感觉这些都不是我想要的，而做为一个 Emacser 不使用 Github 搭建一个自己的博客，感觉就已经落伍了。

于是就有这最初的一个，相当简陋的利用 Emacs 的 Org-mode 搭建的博客，后来看了 Org-page 这个包，但是，我配置不好啊！为什么为什么！

最后，从 Hugo 和 Hexo 之间，我选择了 Hexo，虽然 Hugo 在 Windows 上使用起来更方便，但是我还是觉得 Hexo 更适合我。

## 利用 Hexo 的坑 

1.  有些插件需要翻墙，有些不用，我也不记得哪个用哪个不用了，实在不行可以使用淘宝的
    npm 源进行安装。

2.  环境配置好之后，最好更新一下模板，把一些常用的内容写到 Hexo
    的模板里，这样在后续的使用中，可以快速的增加标签、分类和简介等内容。

3.  学习 Markdown , 这个并不是一个坑，而是一个忠告，作为一个常年游荡在
    GitHub 的好同志来说，但是对于一个 Emacser 来说，我更喜欢 Org-mode
    ，但是 Org-mode 并不能直接用来发布 Hexo
    博客，有些人会说可以利用一些工具，但是与其增加一些工具，不如学习一下
    Markdown, 这根本用不了几分钟，虽然 Org-mode
    很强大（忍不住安利一波），但是 Markdown
    作为一个大众的标记语言，简单的语法还是需要我们掌握的。

4.  记住常用的命令
    - `hexo new markdown_file` 新建文章
    - `hexo new page html_file` 新建页面
    - `hexo generate` 生成静态页面到 public 目录
    - `hexo server` 开启预览访问端口，4000， `Ctrl+c` 关闭 _server_
    - `hexo deploy` 将 .deploy 目录部署到 GitHub

> 这里需要配置 deploy 的项目地址并安装了 `hexo-deployer-git` 插件，才能使用这个功能

1.  最后一个坑，挑选一个合适的主题，好吧，我选择了很久——大概四天吧，可能很多人能非常快的决定，但是对于我来说，把所有好看的主题都看一遍，才是我想做的事，最后我选择了 Next 主题，简单美观，还有非常齐全的配置说明

2.  部署使用的命令有三个 `hexo clean` / `hexo generate` / `hexo deploy`
    ，这三个命令之后，就可以登录你的静态博客页面去查看了。

## 博客的生活 

我很喜欢调试自己的博客，但是写博客就不是那么喜欢了，但是我希望能养成一个定期写博客的习惯。

所以，我需要博客，主要是用来装...咳，主要是用来记录我们的生活、工作的内容，这样在下次使用的时候，就能更好的做到了。

## Hexo 相关安装 

在几次试验之后，Node.js 环境还是使用 nvm
管理比较好用，下载的时候可以使用 `npm --registry=https://registry.npm.taobao.org install` 进行安装下面的模块。

```shell
npm install -g hexo-cli
npm install hexo-deployer-git --save
npm install hexo-generator-search --save
npm install hexo-generator-feed --save
npm install -g tern
npm install -g js-beautify
npm install -g jshint
npm install -g js-yaml
npm install hexo-renderer-jade --save
npm install hexo-renderer-sass --save
```

## hugo


**Hugo** 是由 Go 语言实现的一个 **Static Site Generator** 工具，特点就是快，而且默认是支持 **Org mode** 这种文本的。

虽然对于 **hexo** 而言少了许多好看的主题，但是对于 **Org mode** 的默认支持让我有了决心一用的冲动。

在使用了一段时间之后，发现这个工具完美的解决了我所有的问题，并能让我专心于博客写作本身而不是工具，虽然有一些不方便，但最后还是决定继续使用，而且要减少对工具本身的使用，而加强写作本身。

在长时间的使用之后，发现 **Hugo** 对 **Org mode** 的支持也比较一般，对于一些比较好用的特性，功能都不支持，最好的方案还是从 **Org** 转成 **Markdown** ，所以在最终使用 **ox-hugo** 工具配合 **Hugo** 使用，然后通过 **capture** 功能直接生成对应的博客文章，方便快捷。

### ox-hugo 配置

使用 ox-hugo 主要需要配置两个内容，一是将 **ox-hugo** 增加到配置中，然后是在 启动 org-capture 的时候，增加一个新的选项，可以将自动新增一篇文章。

```emacs-lisp
(use-package ox-hugo
  :after ox)

(with-eval-after-load 'org-capture
  (defun org-hugo-new-subtree-post-capture-template ()
    "Return `org-capture' template string for new Hugo post."
    (let* ((date (format-time-string (org-time-stamp-format :long :inactive) (org-current-time)))
           (title (read-from-minibuffer "Post Title: "))
           (file-name (read-from-minibuffer "File Name: "))
           (fname (org-hugo-slug file-name)))
      (mapconcat #'identity
                 `(
                   ,(concat "* TODO " title)
                   ":PROPERTIES:"
                   ,(concat ":EXPORT_FILE_NAME: " fname)
                   ,(concat ":EXPORT_DATE: " date)
                   ":END:"
                   "%?\n")
                 "\n")))

  (add-to-list 'org-capture-templates
               '("h"
                 "Hugo post"
                 entry
                 (file "~/workspace/blog/hugo-posts.org")
                 (function org-hugo-new-subtree-post-capture-template))))
```

在这里，我是将所有的文章写到对应的一个文件中，然后将文件中的所有内容生成到对应的 hugo 文件夹中。

文件头配置如下：

``` org-mode
#+HUGO_BASE_DIR: ~/workspace/blog/content/
#+SEQ_TODO: TODO DRAFT DONE
#+OPTIONS: ^:{}
```

然后在这个文件中使用导出的快捷键，就可以看到对应的选项了，将 `org-export-dispatch` 命令绑定到自己的快捷键上就可以看到对应的输出命令。

![ox-hugo-export](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/ox-hugo-export.png)
