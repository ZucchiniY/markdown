---
title: Emacs 扩展 Evil 功能
tags:
  - Evil Multiple cursors
  - turn evil mode off
categories:
  - 工具环境
  - Emacs
description: 增加 evil 多光标模式，在某些主模式中关闭 evil。
abbrlink: d51ccb9b
date: 2020-01-12 18:21:13
---

## Evil 多光标模式

今天在修改代码的过程中，发现有一些地方，想使用多光标来修改，但是在使用的时候，感觉不太会用 **evil mc** ，中间切换到了 **multiple-cursors** 包上，但是在 **evil** 模式下使用，因为模式切换的情况，修改代码的时候会弹出一些奇怪的提示，因为模式的切换的问题，所以又换到了 **evil-mc** 上。

如果想要修改一个对应的内容，首先需要进行 **visual** 模式，然后使用 `C-n` 进行选择，然后修改，然后 `grq` 退出功能。

### 常用的快捷键如下： 

`C-n`: 标记当前，找下一个匹配值
`C-p`: 标记肖前，找上一个匹配值
`M-n`: 在已经标记的光标中向后跳转
`M-p`: 向前
`C-t`: 跳过这个，找下一个相同的内容，具体使用过之后，感觉不好用，没有
`grn`: 同上
`grf`: 跳到标记的第一个
`grl`: 跳到标记的最后一个
`grj`: 标记这个位置的的下一行的同一位置
`grk`: 是标记上一行的相同位置
`grs`: 暂停光标移动
`grr`: 恢复光标移动

## 关闭 evil 功能

在最近一段时间的使用过程中，发现 Evil 虽然在某些时候要比 Emacs 的操作更方便，但是在一些 Emacs 的默认使用过程中，还是 Emacs 的更好用，比如说 dired 中。

刚开始希望可以只在 **编辑模式** 中使用 Evil ，比如 org mode 、python mode 这类，但是在配置的时候发现，evil hook 并没有启作用。

```emacs-lisp
(use-package evil
    :hook (org-mode . evil-mode))
```

但是这种方案并不能实现在阅读一些相关文档的过程中发现，可以使用另一个方法来修正这个问题，即在一些特殊的 mode 中关闭 evil 。

```emacs-lisp
(use-package evil
    :config
    (evil-set-initial-state 'dired-mode 'emacs))
```

这样就可以让我们在使用过程中更适合的方式操作了。
