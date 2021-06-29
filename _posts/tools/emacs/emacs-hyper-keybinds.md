---
title: Emacs 中辅助键设置
tags:
  - keymap
  - super
  - hyper
categories:
  - 工具环境
  - Emacs
keywords:
  - meta
  - super
  - hyper
  - control
abbrlink: 3cf89ec9
date: 2019-07-31 08:50:00
---

使用 Emacs 的人，一般都对快捷键的前缀 *C* 和 *M* 键不陌生，但其实在 Emacs 中，除了常见的 *C* 和 *M* 之外，还有 *s* 和 *H* 两个辅助键，但是在不同的操作系统中，辅助键的设置方法也是不一样的，但是我们可以通过在 `init.el` 文件中设置键位来保证快捷键的一致。

- 在 windows 系统下

```emacs-lisp
(setq w32-pass-lwindow-to-system nil)
(setq w32-lwindow-modifier 'super) ; Left Windows key

(setq w32-pass-rwindow-to-system nil)
(setq w32-rwindow-modifier 'super) ; Right Windows key

(setq w32-pass-apps-to-system nil)
(setq w32-apps-modifier 'hyper) ; Menu/App key
```

- 在 Mac 系统下

```emacs-lisp
(setq mac-command-modifier 'meta) ; make cmd key do Meta
(setq mac-option-modifier 'super) ; make opt key do Super
(setq mac-control-modifier 'control) ; make Control key do Control
(setq ns-function-modifier 'hyper)  ; make Fn key do Hyper
```

在如此配置之后，绑定快捷键过程中，super 对应的是 *s* 前缀，hyper 对应的是 *H* 的前缀。

```emacs-lisp
(global-set-key (kbd "H-b") 'backward-word) ; 绑定的 Hyper 键
(global-set-key (kbd "s-b") 'backward-word) ; 绑定的 super 键
```
