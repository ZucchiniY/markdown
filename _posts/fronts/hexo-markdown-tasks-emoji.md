---
title: Hexo 中使用 emoji 和 tasks
tags:
  - markdown-it
  - emoji
  - tasks
categories:
  - 工具环境
  - hexo
description: 让 hexo 博客的 markdown 文件中支持 emoji、tasks。
abbrlink: 953e2b
date: 2020-01-11 21:06:58
---

## 替换为 markdown-it

今天在迁移博客项目的时候，发现原来在 hugo 中可以使用的 Emoji 和 tasks 功能都不能正常使用了，查询了一下原因，主要是因为 hexo 默认的解析器是 `hexo-renderer-marked` ，这个默认的渲染器是不支持 emoji 功能的，但是支持 tasks，但是这个渲染器是不支持扩展的，所以如果希望同时使用这两个功能的话，就需要换一个渲染器。

这里推荐的是 `hexo-renderer-markdown-it` 渲染器，支持扩展，采用的是 `markdown-it` 的内核来解析 markdown 的文本。

```shell
npm un hexo-renderer-marked -S
npm i hexo-renderer-markdown-it -S
```

## 安装和配置 markdown-it

这样就替换完成了，然后再安装需要的插件：

`npm i markdown-it-emoji markdown-it-task-lists -S`

然后再增加相关配置：

```shell
markdown:
    render:
        html: true # 在 markdown 文本中支持 html tag 标签
        xhtmlOut: false # 需要 xtml 文档，使用 <br /> 替代 <br>
        breaks: true # 用 <br> 开始新的一行
        linkify: true # 自动将 可能是链接的内容转换成链接
        typographer: true # 印刷标识转换
    plugins:
        - markdown-it-abbr
        - markdown-it-footnote
        - markdown-it-ins
        - markdown-it-sub
        - markdown-it-sup
        - markdown-it-emoji 
        - markdown-it-task-lists
    anchors:
        level: 2
        collisionSuffix: ''
        permalink: false,
        permalinkClass: 'header-anchor'
        permalinkSymbol: ''
        case: 0
        separator: ''
```

typographer 解释：

将 `(c) (C) (r) (R) (tm) (TM) (p) (P) +-` 这些标识转换成  © © ® ® ™ ™ § § ± 。

一些常用的插件，比如上标和下标，可以在插件里加上 `markdown-it-sub` 和 `markdown-it-sup` ，可以直接用 `19^th^` 19^th^ 还有 `H~2~O` 表示 H~2~O 。

还有脚本、定义列表等功能，具体的见 [https://markdown-it.github.io/](https://markdown-it.github.io/) 。

## 其它插件

因为 markdown-it 是支持扩展的，所以怎么找对应的扩展，也是非常重要的功能，比如 tasks 的支持，可以到 [https://www.npmjs.com/](https://www.npmjs.com/) 里进行搜索，关键字是 `keywords:markdown-it-plugin` 或者直接打开链接 [https://www.npmjs.com/search?q=keywords:markdown-it-plugin](https://www.npmjs.com/search?q=keywords:markdown-it-plugin) 。

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/hexo-markdown-it-task-lists.png)

就可以按照对应的功能去找寻找插件了。
