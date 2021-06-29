---
title: Matplotlib 的使用及问题解决
tags:
  - Matplotlib
categories:
  - 后台技术
  - Python
description: 解决 matplotlib 下中文字体乱码的问题和 conda 的国内源使用方法。
abbrlink: 1c5c5ae6
date: 2021-03-16 15:08:00
---

## 安装 

安装的时候最好选择将 **anaconda** 加入到环境变量中，这样可以直接使用 `conda` 命令来管理包，而不需要增加额外的配置。

## 国内源镜像 

国内使用的话，镜像就还是用 **清华大学开源软件镜像站** ，按步骤安装：

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

就可以了，如果是 `conda` 不包含的库的话，还是需要使用 `pip` 命令进行安装的，可以同样配置成清华源

```shell
# 临时使用
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
# 设为默认值
pip install pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
# 但是这个需要 pip 版本在 10.0.0 以上，如果低的话，可以临时升级
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U
```

## 包管理 

```shell
conda list #查看所有的包
conda install package_name #安装包
conda remove package_name #移除包
conda update package_name #升级包
conda search search_term #模糊查询包名
conda update conda #更新 conda 本身
conda update anaconda #更新 anaconda
conda update python #更新 Python
```

## 中文乱码

使用 Anaconda 进行数据处理后生成图片的时候，如果不指定对应字体会导致中文乱码，可以通过下面的方案进行解决。

```python
from pylab import mpl
# 指定默认字体：解决plot不能显示中文问题
mpl.rcParams['font.sans-serif'] = ['Microsoft YaHei']
# 解决保存图像是负号'-'显示为方块的问题
mpl.raParams['axes.unicode_minus'] = False
```

## Mac 下查找字体

可以利用 `font_manager` 将所有 matplotlib 能使用的字体打印出来。

``` python
from matplotlib import font_manager
a = sorted([f.name for f in font_manager.fontManager.ttflist])
for i in a:
    print(i)
```

如果 matplotlib 中无中文字体，则需要下载对应的 tff 字体只在到 matplotlib/mpl-data/fonts/tff 中，然后使用 font_manager 重新编译一下。

```python
from matplotlib.font_manager import _rebuild

_rebuild()
```

之后就可以利用上面的方法，将使用新的字体了。

```python
plt.rcParams['font.sans-serif'] = ['STFangsong']
plt.rcParams['axes.unicode_minus'] = False
```
