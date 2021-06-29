---
title: Emacs 学习之旅
tags:
  - 总结
  - Emacs
  - org mode
categories:
  - 生活总结
keywords:
  - Emacs
  - org mode
  - Prelude
  - Purcell
  - Spacemacs
description: 学习 Emacs 要从使用开始，而不是配置。只有使用到了，再增加相应的配置，才是最好的配置。
abbrlink: ba9b36be
date: 2017-03-02 06:38:00
---

**Emacs 的使用过程，就像是程序员的生涯一样——路漫漫其修远兮，吾将上下而求索。**


## 万物始于 Emacs

最早知道 **Emacs** 是从编辑器的圣战开始的，即编辑器之神——Vi，和神的编辑器——Emacs。两个编辑器在经历了几十年的战争之后，仍然是编辑世界不可超越的高峰。

但在一开始，我选择的是 Vi，因为在 **\*nix** 中，都是有安装的，在服务器编辑文件——即使是很大的文件，Vi 也可以非常轻易的打开编辑，在一段时间内，我几乎是跪着使用 Vi 的。

后来随着想用的功能越来越多，而 Vi 只能做为编辑器使用，再加上被一些大神安利，我就选择尝试使用 Emacs 来~装逼~记笔记。于是下载了当时正流行的 _Purcell_ 大神的配置，并开始尝试使用，不过没过多少就放弃了。

期间阅读了许多入门学习的内容，对 Emacs 有了一个大概的了解。

推荐阅读内容：

- [《一年成为 Emacs 高手（像神一样使用编辑器）》](https://github.com/redguardtoo/mastering-emacs-in-one-year-guide/blob/master/guide-zh.org)
- [Prelude 入门级 Emacs 配置](https://github.com/bbatsov/prelude)
- [Purcell 大神的配置](https://github.com/purcell/emacs.d)

## Emacs 始于 Org

Emacs 学习的无疾而终，让我的装逼大计一度沉沦。直到我开始尝试利用 **Org-mode** 进行博客写和作日程管理，阅读了一些文章之后，才真正开始了 Emacs 的学习苦旅。

如果说 Emacs 是神的编辑器的话， **Org** 可能是神器之中的神器，随着对 Org 的学习和使用，我从最初的装逼，到后来的~逼格提升~真正开始利用Emacs，都是因为Org-mode 。

推荐阅读内容：

- [mudan 大神的 Org-mode 入门级手册](https://github.com/mudan/mudan.github.io/blob/master/Emacs/The%5FOrg%5FManual/The%5FOrg%5FManual.org)
- [mudan 大神的漂亮的文言文排版](https://github.com/mudan/mudan.github.io/blob/master/read/dx.org)
- [Tisoga 大神的 Org + GitHub 的博客教学](http://forrestchang.com/14824097554043.html)

## 终于 Spacemacs 的战争

从最开始的学习，到现在已经习惯于使用 Emacs ，主要因为其确实是可以提升效率的，当然这里要把配置时间拿走。虽然开始使用的原因有所不同，但是大家最后的目标却都是一样的——即提高工作（学习）效率。

但是经过了 Emacs 几次强行配置之后，学习了一些 Emacs 的填坑方案。

后来加入了一个 Emacs 的微信群——毫不夸张的说，这是我加入过的群里面质量最高的，学习效果最好的，而且所有的成员都自发的维护群里的闲聊问题，每一次讨论都是提问解决和讨论的过程。

在偶然的一次机会，被安利了一把 Spacemacs，Vi 的操作加上 Emacs 的扩展，不要太吸引人！

推荐关注的大神：

- [Hick](https://github.com/hick) 高质量 **Emacs** 微信群群主，应该也是发起人，水的人自觉加入闲聊群，是我所有技术相关微信群中质量最高的。
- [子龙山人](https://github.com/zilongshanren) **Spacemacs Rock** 视频作者，我的配置里抄的最多的就是这位大神的。
- [DarkSun](https://github.com/lujun9972)  黑日大神，大神的文章非常多，而且质量都非常高，还维护着一个 Emacs 推广相关的项目，多读读，可以找到一些自己需要的配置。
- [tumashu](https://github.com/tumashu) 天然二呆，呆神，之前看到呆神在闲聊群里水，后来又看到呆神在帮忙解决问题，好奇的关注了一下 GitHub ，才发现，竟然这几个好用的 package 都是呆神写的，而且呆神竟然不是~程序员~靠程序吃饭！

大神太多了，不一一推荐，如果需要，可以联系 Hick 加一下群，就都有了。

再推荐一下中文的 Emacs 论坛，可以提问，也可以讨论：

- [Emacs China](https://emacs-china.org) 一堆大神在维护的论坛，经常看看，非常好用。


## 我的 Emacs 配置


### 初始

为了更好管理配置，推荐使用 **.spacemacs.d** 文件夹进行管理配置，而不是使用 **.spacemacs** 文件。也为了方便后续的扩展。


### 可能会遇到的问题

如果是在 Windows 下使用，需要注意几个问题：

1.  推荐用编译版本，或者用官方网站加部分 *.dll* 文件来解决
2.  使用过程中，为了配置的时候好用——更适合 Linux，我是使用在环境变量中增加默认的 *HOME* 的方案，也可以使用其它方法
3.  直接下载就可以使用，维护的是 *develop* 分支，后续会慢慢往 *master* 分支中合并


### 最终选择

在几经周折之后，最后还是选择自己从头开始配置一套 **.emacs.d** ，主要是因为以下几个问题：

1. 随着使用的人越来越多，维护的东西也越来越多，项目太大了
2. 最终希望的是使用 Vi 的快捷键方案，可以使用 `evil-mode` 来替代
3. 个人使用的特性话的内容太多，完全引用项目不如借鉴项目的配置方案

[我的 emacs 原生配置](https://github.com/AboutEmacs/.emacs.d)


### 我的博客地址

如果想看我的博客，可以访问：[hugo博客](https://www.zucchiniy.cn) 或者 [hexo博客](https://zcodingtime.github.io)。
