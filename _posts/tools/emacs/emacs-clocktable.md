---
title: 统计每天的工作时长
Tags:
  - org mode
  - clocktable
categories:
  - 工具环境
  - Emacs
description: 利用 Org mode 中的 clocktable 统计自己的的工作时长。
abbrlink: 12c28564
date: 2020-01-14 15:53:21
---

作为一个 Emacs 的使用者，一直都希望可以完全的使用 Emacs 进行时间管理，而作为时间管理中的重头戏——番茄时间，在 Org 也是一个常用的功能。

按自己的工作专注程序，我用的更多的是 clock-in 和 clock-out 的功能来记录时间。然后在每周结束的时候，进行时间任务的复盘，虽然不能把所有的任务都放到 Org 中管理，但至少和电脑相关的任务都可以这么记录。

复盘的时候需要看在一段时间之内，到底都做了哪些事情，这里就需要用到 Org mode 中的 Clocktable 功能，对应的命令是 `org-clock-report` ，在这之后会生成一个对应的的时间表格：

```org
#+CAPTION: Clock summary at [2020-01-14 Tue 16:02], for January 2020.
| File              | Headline                | Time       |          |
|-------------------|-------------------------|------------|----------|
|                   | ALL *Total time*        | *2d 13:40* |          |
|-------------------|-------------------------|------------|----------|
| tasks.org         | *File time*             | *2d 13:40* |          |
|                   | Tasks                   | 2d 9:07    |          |
|                   | \_  Emacs 配置          |            | 0:29     |
|                   | \_  调整 hexo 博客主题  |            | 1d 10:21 |
|                   | \_  markdown-it-plugins |            | 22:17    |
|                   | Habits                  | 4:33       |          |
|                   | \_  英语单词            |            | 4:33     |
|-------------------|-------------------------|------------|----------|
| tasks.org_archive | *File time*             | *0:00*     |          |
```

可以看到，这个是按文件进行的统计，目前都是在 task.org 文件中的任务。这里也可以选择按自己的想法进行统计。

```text
#+BEGIN: clocktable :scope agenda-with-archives :block thismonth :maxlevel 2
```

这里主要是几个参数：

`scope`: 统计范围，可以按文件统计，也可以按其它的范围进行统计。

`block`: 时间跨度，可以是今天 `today` 本星期 `thisweek` 或者是本月 `thismonth`,当然也可以是今年 `thisyear`，或者是按季度进行统计 `2020-Q1` 当然也支持按星期统计 `2020-W2` 

`step`: 跨度，可以是天 `day`，星期 `week`，年 `year`

其它的参数可以参照[The clock table](https://orgmode.org/manual/The-clock-table.html)，常用的主要就是这几个参数。

这些配置之后，就可以看天看自己到底花了多少时间在正经事儿上了。
