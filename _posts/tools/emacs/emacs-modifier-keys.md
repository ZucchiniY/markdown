---
title: Emacs 功能键配置
tags:
  - modifier keys
categories:
  - 工具环境
  - Emacs
keywords:
  - modifier keys
  - w32-lwidow-modifier
  - mac-command-modifier
description: 在不同操作系统上绑定 Emacs 功能键。
abbrlink: 749f099
date: 2019-02-26 06:10:00
---

Emacs 和 Vim 最大的就是快捷键的体系不同，在 Emacs 中，快捷键要有对应的控制键配合，才能正常使用，比如打开 **Agenda** `C-c a` 一般指的是 `Ctrl + c a` 而在 Emacs 中，使用的控制键主要有以下几种：

```text
s- : supper
S- : Shift
M- : Meta / Alt
C- : Ctrl
H- : Hyper
```

其中 Ctrl、Meta/Alt、Shift这几种快捷键比较常见，但是 supper 这个键就比较少见了，而且在键盘上，一般也看不到，所以我们在配置的时候，需要在配置中声明这几个键被绑定在哪些键上。

如果是在 /Windows/ 系统下需要增加如下的配置：

```emacs-lisp
(setq w32-lwindow-modifier 'supper
    w32-apps-modifier 'hyper)
```

但是如果使用的是 /Mac/ 系统的话要增加如下配置：

```emacs-lisp
(setq mac-command-modifier 'meta
  mac-option-modifier 'super
  mac-control-modifier 'control
  ns-function-modifier 'hyper)
```

但是这样的情况又有另外一个问题，需要在特定的系统中使用，所以我们要在对应的配置上增加上对系统的判断，绑定的方案如下：

```emacs-lisp
(when sys/winntp
  ;; 经过测试，在 windows 下，window 键是不能修改的
  (setq ;;w32-lwindow-modifier 'supper
	    w32-apps-modifier 'hyper)
  (w32-register-hot-key [s-t]))

(when sys/macp
  (setq mac-command-modifier 'meta
	    mac-option-modifier 'super
	    mac-control-modifier 'control
	    ns-function-modifier 'hyper))
```

这样我们就可以在不同的系统中正确的使用不同的功能键了。
