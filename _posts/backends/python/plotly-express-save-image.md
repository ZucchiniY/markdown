---
title: Plotly_express 保存图片
tags:
  - Plotly express
  - 保存成图片
categories:
  - 后台技术
  - Python
description: 如何在使用 plotly_express 将图片保存到本地。
abbrlink: 3f467d49
date: 2021-03-18 16:26:00
---


在使用 Plotly_express 快速绘图的时候，希望将生成的图片保存到本地。

首先需要安装必须的配置，因为我是使用的 anaconda 所以使用 Conda 进行安装。

``` python
conda install -c conda-forge python-kaleido
```

如果是使用的 pip 管理的话，可以使用 `pip install -U kaleido` 直接安装。

同时使用 `fig.write_imgage('xxx.png')` 来保存图片。

``` ptyhon 保存图片
import plotly.graph_objects as go
fig = go.Figure()
fig.write_image("image.png")
```

如果希望保存的是其它类型的图片，则可以修改对应的后缀，包括 **png/jpeg/wdbp/svg/pdf** 等类型。

如果使用其它引擎进行图片生成的话，可以使用 `fig.to_image(format='png', engine='kaleido')`进行生成。
