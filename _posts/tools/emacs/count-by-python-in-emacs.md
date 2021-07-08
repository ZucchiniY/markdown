---
title: 在 Emacs 中统计表格中的记录
icons:
  - fas fa-fire red
  - fas fa-star green
abbrlink: bbd91aba
date: 2020-12-01 13:52:02
updated: 2021-07-08 13:52:02
tags:
  - Emacs
  - Python
categories:
  - 工具环境
  - Emacs
keywords:
  - python
  - plotly_express
  - Emacs
description: 在 Emacs 中通过 Python 计算表格中的数据并绘图。
---

在 Emacs 中利用 clocktable 统计了一段时间内的详细时间，想看看自己对时间的利用效率。但是在绘图和计算的时候遇到了问题，Emacs 可以直接使用一些代码来绘制图表，但是使用过程中不并是特别方便，相对于这些图表，更希望使用常用的 Python 进行管理。

`#+TBLFM:` 是用来计算表格的，是 org mode 中可以直接使用的运算，主要是将时间全都自动转换成分进行计算，如果不转换成分钟的话，会默认按日来统计结果，未找到具体的原因。

```org 数据表
#+name: timedate
| Headline  | day | hour | min |   all |
|-----------+-----+------+-----+-------|
| work      |   6 |    2 |  37 |  8797 |
| waste     |  13 |    7 |   7 | 19147 |
| commuting |   4 |    4 |   3 |  6003 |
| pleasure  |  10 |   19 |  14 | 15554 |
| life      |   8 |   11 |   6 | 12186 |
#+TBLFM: $5=$2*24*60 + $3 * 60 + $4;
```


```org Python 的绘图代码
#+begin_src python :var data=timedate :results output :python /Users/tech/opt/anaconda3/envs/org_env/bin/python

  # import sys
  # print(sys.version)
  # print(sys.prefix)
  import pandas as ed
  import numpy as np
  import plotly_express as px
  import os

  fig = px.pie(data, names=0, values=4)
  fig.write_image('gtd.svg')
#+end_src
```

其实就是利用 Python 的 plotly_express 包来绘制一个饼图。
