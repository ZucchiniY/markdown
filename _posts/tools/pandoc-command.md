---
title: Pandoc 命令
tags:
  - org
  - docx
  - markdown
categories:
  - 工具环境
  - Pandoc
description: Pandoc 转换文档格式。
abbrlink: '4032e239'
date: 2016-01-19 09:08:00
group: notes
---

## org 转换为 docx 

-   基本命令

```shell
pandoc xxx.org -o xxx.docx
```

-   利用 css 进行配置着色

```shell
pandoc 01-chapter2.markdown -o chapter2.docx -c Github.css
```

## org 转换为 letex 

-   使用指定字体

```shell
pandoc pandocCh.org -o pandocCh.pdf --latex-engine=xelatex -V mainfont="SimSun"
```

-   使用指定模板

```shell
pandoc pandocCh.org -o pandocCh.pdf --latex-engine=xelatex -template=pm-template.latex
```

## org 转换为 html 

```shell
pandoc 01-chapter2.org -o chapter2.html -c Github.css ```
```

## org 转换为 Markdown 

```shell
pandoc -f org -t markdown -o output.md input.org
```
