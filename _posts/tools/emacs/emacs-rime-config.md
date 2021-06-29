---
title: Emacs中配置使用Rime输入法
tags:
  - librime
  - emacs-rime
categories:
  - 工具环境
  - Emacs
description: 在Emacs中使用Rime输入中文。
top: true
abbrlink: 2f2b0c30
date: 2020-04-21 14:26:59
---


在 Emacs 中使用外部输入法，最大的问题是在切换 evil 的模式的时候，对输入来说会有延迟，因为需要手工将输入法切换到对应的英文模式，才能正常使用快捷键。

但是如果使用的是 Emacs 自己的输入功能，则不需要做这些同步，只需要从 insert 模式中退出即可，这种操作对 Emacs 来说真的是太方便了。

之前使用的是 pyim + liberime 的方案，在今天更新了配置之后，无法再使用这个方案，调整配置之后，也无法正常使用，经过测试，将配置调整为 emacs-rime 的方案。

首先需要下载对应的内容：[librime](https://github.com/rime/librime/releases/download/1.5.3/rime-1.5.3-osx.zip)。

将解压之后的内容，放到 user-emacs-directory 路径下，然后增加配置。


```emacs-lisp
(use-package rime
  :config
  (setq rime-show-candidate 'posframe)
  :custom
  (rime-librime-root (expand-file-name "librime/dist" user-emacs-directory))
  (rime-emacs-module-header-root (expand-file-name "extends" user-emacs-directory))
  (default-input-method "rime"))
```

报错：Can’t find rime_api.h when compile

```emacs-lisp
(rime-librime-root (expand-file-name "librime/dist" user-emacs-directory))
```

报错：Can’t find emacs-module.h when compile

先将 */Applications/Emacs.app/Contents/Resources/include/emacs-module.h* 文件放到 *.emacs.d/extends/* 下。

```emacs-lisp
(rime-emacs-module-header-root (expand-file-name "extends" user-emacs-directory))
```

经过这样的配置之后，就能非常容易的在 Emacs 中使用 rime 输入法了。
