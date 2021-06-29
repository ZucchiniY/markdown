---
title: Emacs 快捷键配置方案
tags:
  - keybinds
categories:
  - 工具环境
  - Emacs
description: Emacs 中快捷键的配置和选择。
abbrlink: 86598d2e
date: 2020-01-08 00:00:00
---

**Emacs 的快捷键和 Vim 的快捷键是编辑器中的两坐高山，其中 Emacs 的快捷键主要有四类。**

## 四大类型 

-   全局快捷键

```emacs-lisp
(global-set-key (kdb "a") 'command)
```

-   全局映射键

```emacs-lisp
(define-key key-translation-map (kbd "a") (kdb "b"))
```

-   Major-mode 局部快捷键

```emacs-lisp
(local-set-key (kdb "a") 'command)
```

-   Minor-mode 局部快捷键

```emacs-lisp
(define-key your-minor-mode-map (kbd "a") 'command)
```

## 删除、禁用快捷键 

```emacs-lisp
(global/local-unset-key (kbd "a"))
(global/local-set-key (kbd "a") 'ignore/nil)
```

## 键冲突与解决 

最方便的解决方案是找一个空置的 **prefix** 键，先映射到这个键上，再全局或者局部设置它。

### 先映射到空闲键上 

```emacs-lisp
(define-key key-translation-map (kbd "a") (kbd "M-g A"))
```

### 全局或者局部设置 

```emacs-lisp
(global/local-set-key (kbd "M-g A") 'command)
```

## 快捷键优先级 

`key-translation-map` : 最高级，就是把这个键的意义改变了，想使用原来的快捷键，要重新进行绑定

`minor-mode-map` : 二级，只在 **minor mode** 激活时启作用，其它时候会被其它的快捷键覆盖掉

`local-set-key` : 三级，在 **major mode** 中启作用

`global-set-key` : 最弱的级别，但是也是最简单的键绑定方式


### 设置局域快捷键 

```emacs-lisp
(defun f-python-mode ()
    (local-set-key (kbd "C-x C-e")'f-python-shell-send-line)
    (local-set-key (kbd "M-g C-y") 'f-python-shell-send-line))
(add-hook 'python-mode-hook 'f-python-mode)
```

**注意** 当键进行重新绑定后，还应该将之前的功能重新绑定到另一个键上。

## Minor Mode Map 

```emacs-lisp
(define-minor-mode visual-mode
  :init-value nil
  :global t
  :keymap (make-sparse-keymap)
  (if (not visual-mode) (setq cursor-type 'bar)
    (setq cursor-type 'box)))
(define-key visual-mode-map (kbd "h") 'mark-paragraph)
```

定义之后，可以利用 `define-key` 来设置当前快捷键。然后在需要启用 **Visual mode** 的时候可以启用这个 **minor mode** 的相关快捷键。
