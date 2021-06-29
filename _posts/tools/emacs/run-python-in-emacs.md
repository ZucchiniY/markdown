---
title: 在 Emacs 中执行 Pyhton
tags:
  - org mode
  - Python
categories:
  - 工具环境
  - Emacs
description: 在 Org mode 中执行 python 代码。
abbrlink: d07fab41
date: 2019-08-07 07:18:00
---

最近在整理 Python 的相关的内容，主要需要整理成笔记，记录下来，等有需要的时候再进行复习。

在编写 _org_ 的时候，发现 **Python** 的内容并不能很好的执行，而且生成的图片也不能正常显示，所以查询了一下资料，发现如果是 **python** 的话，需要按下面的形势处理：

```text
#+BEGIN_SRC python :results file :preamble "# -*- coding: utf-8 -*-" :python python3 :exports both
```

其中 `:results` 针对不同的执行结果进行调整，如果是想把 Python 生成的图片显示在 org 文档里的话，就要选择 file ，如果是想显示执行的结果的话，就使用 output 。

`:preamble` 的话，是针对 Python 的码制了，现在如果有中文的话，可能需要指定为 utf-8 所以默认需要加上这个内容。

`:python` 是用来指定解释器的，在 Mac 环境下，执行的时候，总是提示找不到 pandas 但是如果直接使用 `python test.py` 的话是能正常显示结果，可能是因为默认查找的 python2 吧，这里进行指定到 python3 上就可以使用了。

`:exports` 是指定输出的情况的，code 是指显示代码，results 是指的仅显示结果，both 是两个都显示，none 则是指的都不显示。

`:session` 是特殊情况，有些时候需要调用方法中的 return 使用 session 的话能直接使用，可以不必再单独返回了。

`:var` 可以指定传入的参数

使用示例如下：

```org-mode
#+tblname: data_table
| a | 1 |
| b | 2 |
| c | 3 |
#+begin_src python :var val=1 :var data=data_table
return(data[val])
#+end_src

#+RESULTS:
| b | 2 |

#+begin_src python :results file
import matplotlib, numpy
matplotlib.use('Agg')
import matplotlib.pyplot as plt
fig=plt.figure(figsize=(4,2))
x=numpy.linspace(-5,5)
plt.plot(numpy.sin(x)/x)
fig.tight_layout()
plt.savefig('./images/python-matplot-fig.png')
return './images/python-matplot-fig.png' # return filename to org-mode
#+end_src

#+RESULTS:
[[file:./images/python-matplot-fig.png]]
```

将这个内容增加到 snippet 中去，在 snippet/org-mode/ 路径下增加 python
文件，其中内容如下

```snippet
# -*- mode: snippet -*-
# name: python
# key: <pyt_
```
