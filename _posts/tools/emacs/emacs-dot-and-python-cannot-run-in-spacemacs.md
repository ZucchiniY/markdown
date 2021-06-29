---
title: Org mode 中不能执行 dot 、 Python 代码
tags:
  - spacemacs
  - org babel
categories:
  - 工具环境
  - Emacs
keywords:
  - emacs
  - spacemacs
  - org babel
description: 在 Spacemacs 中的 org mode 模式下，无法直接执行代码段时，需要重新编译 elpa 模块。
abbrlink: b203efa6
date: 2017-07-31 03:23:00
---

- 无法执行的代码

更新之后，**dot** 、 **plantuml** 的代码段在 **Org-mode** 下无法执行，需要引入对应的 **ob-xxx.el** 才能正常执行。

可以手工重新编译或者重新下载 **Org** 相关 **package** 即可，也可以使用下面的命令进行更新。

```emacs-lisp
:spacemacs/recompile-elpa
```
