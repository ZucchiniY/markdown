---
title: Emacs Org mode 小技巧
tags:
  - org mode
  - use-sub-superscripts
  - ditaa
categories:
  - 工具环境
  - Emacs
description: 增加转义扩展符、ditaa 语言绘制图形。
abbrlink: 5f8ba428
date: 2020-01-12 18:38:24
---

## 启用转义符转义

在 Org Mode 写一些笔记的时候，经常会用到下划线 `-`，而 `a_b` 总是会变成 a~b~ 的形式，可以通过在文档的最上面，增加配置来关闭自动转义，对于在文章头部加上了 `#+OPTIONS: ^:nil` , 还可以通过配置 `(setq org-use-sub-superscripts nil)` 的方式来实现全局配置。

## 用 ditaa 绘制图形

```ditaa
#+BEGIN_SRC ditaa :file ../images/linux-os.png :exports both :cmdline -E -r -s 1.0
+---------------------------------------------------------+
|                Applications                             |
|    +----------------------------------------------------+
|    |           System Libraries                         |
+----+----------------------------------------------------+
|                System Call Interface                    |
+------------------------+--------------+-----------------+       +---------+
|          VFS           |   Socket     |                 |       |         |
+------------------------+--------------+    Scheduler    +-------+   CPU   |
|       File Systems     |   TCP/UDP    |                 |       |         |
+------------------------+--------------+-----------------+       +----+----+
|       Volume Manager   |   IP         |    Virtual      |            |
+------------------------+--------------+    Memory       |            |
| Block Device Interface |   Ethernet   |                 |            |
+------------------------+--------------+-----------------+       +----+----+
|                       Device Driver                     |       |  DRAM   |
+-----------------------------+---------------------------+       +---------+
                              |
                              |
                      +-------+--------+
                      |   I/O Bridge   |
                      +-------+--------+
                              |
                              |
      ------+-----------------+--------------------+------
            |                                      |
  +---------+--------+                  +----------+---------+
  |  I/O Controller  |                  | Network Controller |
  +-+-------+------+-+                  +----+----------+----+
    |       |      |                         |          |
+---+---+   |  +---+---+                +----+----+ +---+----+
| Disk  |  ... | Swap  |                |  Port   | |  Port  |
+-------+      +-------+                +---------+ +--------+
```

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/linux-os.png)
