---
title: Surround 笔记
tags:
  - evil
  - surround
categories:
  - 工具环境
  - Emacs
keywords:
  - evil
  - surround
description: Emacs 中使用 Vi 的 surround 插件。
abbrlink: 3873eb44
group: notes
date: 2018-02-12 09:09:00
---

## surroud 插件 

项目的地址如下

- [vim surround](https://github.com/tpope/vim-surround)

| 原文本                 | 命令     | 新文本                    |
|------------------------|----------|---------------------------|
| "Hellow world!"        | ds"      | Hellow world!             |
| [123+456]/2            | cs])     | (123+456)/2               |
| "Look ma, I'm \*HTML!" | cs"<q>   | <q>Look ma, I'm HTML!</q> |
| if  x > 3 {            | ysW(     | if( x>3 ) {               |
| my $str = whee!;       | vllllS'  | my $str = 'whee!';        |
| `<div>Yo!</div>`       | dst      | Yo!                       |
| `<div>Yo!</div>`       | `cst<p>` | `<p>Yo!</p>`              |

上面的示例中，添加成对的括号时，如果使用后半括号，是没有空格的，如第 2 个示例，如果使用前半个括号，则是有空格的，如第 4 个示例。另外对于一些常见的标记，需要记住：

1.  t 表示 xml 或者 html 中的 Tag
2.  w word
3.  W WORD
4.  p paragraph

## 命令表格 

### Normal mode 

`ds` : 删除一对配对符号

`cs` : 替换原来的配对符号

`ys` : 加一对配对符号

`yS` : 增加一对配对符号，并将内容新建一行，并缩进

`yss` : 为整行增加一对配对符号

`ySs` : 为整行增加一对配对符号，并新起一行，然后缩进

`ySS` : 同 ySs

### Visual mode 

`s` : 增加一对匹配符号

`S` : 增加一对匹配符号，并新起一行，然后缩进


### Insert mode 

`C-s` : 增加一对匹配符号

`C-s C-s` : 增加一对匹配符号，并新起一行，然后缩进

`C-g s` : 增加一对匹配符号

`C-G S` : 增加一对匹配符号，新起一行然后进行缩进

## 修改 `surrounding` 内文本为例： 

`ci` : 修改匹配符号内的文本，并进入插入模式

`di` : 剪切匹配符号之间的文本

`yi` : 复制匹配符号之间的文本

`ca` : 同 `ci` 但是也修改符号本身

`da` : 同 `di` 但是也修改符号本身

`ya` : 同 `yi` 但是也修改箱号本身

>**b 可以表示小括号，B 表示大括号**
