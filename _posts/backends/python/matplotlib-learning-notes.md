---
title: Matplotlib 学习笔记
tags:
  - Matplotlib
categories:
  - 后台技术
  - Python
description: matplotlib 常用图标示例。
abbrlink: c2ad7d73
date: 2019-07-30 02:38:00
---

记录了几个好入的可视化库，学习还是要从基础—— Matplotlib 开始学习。

## 图表基本 

```python
import numpy as np
import matplotlib.pyplot as plt
plt.plot(np.random.rand(10))
# 创建图表
plt.show()
```


![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib01.png)

与 Emacs org mode 交互使用：

```python3
import matplotlib.pyplot as plt
import matplotlib
import numpy
matplotlib.use('Agg')

fig = plt.figure(figsize=(4, 2))
x = numpy.linspace(-15, 15)
plt.plot(numpy.sin(x)/x)
fig.tight_layout()
plt.savefig('images/python-matplot-fig.png')
return '/images/python-matplot-fig.png'  # return filename to org-mode
```

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/python-matplot-fig.png)

`plt.close()` : 关闭窗口

`plt.gcf().clear()` : 每次清空图标内的内容

## Matplotlib 图例 

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib-anatomy.png)

## 折线图 

```python
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib
import numpy as np

df = pd.DataFrame(np.random.rand(10, 2), columns=['A', 'B'])
f = plt.figure(figsize=(10, 10))
fig = df.plot(figsize=(8, 6))

# 表头
plt.title('aa')
plt.xlabel('x')
plt.xlabel('y')

# 图例位置
# best 自适应位置
# upper right
# upper left
# lower left
# lower right
# right
# center left
# center right
# lower center
# upper center
# center
plt.legend(loc='best')

# x 轴边界
plt.xlim([0, 10])
# y 轴边界
plt.ylim([0, 1.1])
# 设置 x 刻度
plt.xticks(range(10))
# 设置 y 刻度
plt.yticks([0, 0.2, 0.4, 0.6, 0.8, 1.0, 1.2])
# x 轴刻度标签
fig.set_xticklabels('%.1f' % i for i in range(10))
# y 轴刻度标签
fig.set_yticklabels('%.2f' % i for i in [0, 0.2, 0.4, 0.6, 0.8, 1.0, 1.2])
# 边界限定了值的范围，刻度表示显示的标尺，这里 x 轴是 0 - 10 ，但是刻度只有 0.0 - 9.0
plt.savefig('./images/matplotlib02.png')
return '/images/matplotlib02.png'
```

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib02.png)

-   图表基本样式

    :::python
    import numpy as np
    import matplotlib.pyplot as plt
    import matplotlib
    
    x = np.linspace(-np.pi,np.pi ,256, endpoint=True)
    c, s = np.cos(x), np.sin(x)
    
    plt.plot(c)
    plt.plot(s)
    
    # 显示网格
    # linestyle: 线型
    # color: 颜色
    # linewidth: 宽度
    # axis: x,y,both 显示 x,y,两者
    plt.grid(True, linestyle='--', color='gray', linewidth='0.5', axis='both')
    plt.tick_params(bottom='on', top='on', left='on',right='on')
    # 显示刻度的方向 in, out, inout
    matplotlib.rcParams['xtick.direction']='out'
    matplotlib.rcParams['ytick.direction']='in'
    # 返回当前 axes 对象，gcf() 返回当前 figure 对象
    frame = plt.gca()
    plt.axis('on')
    frame.axes.get_xaxis().set_visible(True)
    frame.axes.get_yaxis().set_visible(False)
    
    plt.savefig('./images/matplotlib03.png')
    return '/images/matplotlib03.png'

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib03.png)

-   线样式
    -   `-`: 直线
    -   `--`: 虚线
    -   `-.`: 点横线
    -   `:`: 全点线


## 子图 

在 matplotlib 中，整个图像为 Figure ，而一个 Figure 中可以有多个 axes。

    :::python
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import matplotlib
    matplotlib.style.use('bmh')
    
    fig = plt.figure(figsize=(10, 6), facecolor='gray')
    # 创建图表，在2行2列的第一个位置
    ax1 = fig.add_subplot(2, 2, 1)
    plt.plot(np.random.rand(50).cumsum(), '--g')
    
    ax2 = fig.add_subplot(2, 2, 4)
    ax2.hist(np.random.rand(50).cumsum(), alpha=0.5, color='b')
    
    ax4 = fig.add_subplot(2, 2, 2)
    df2 = pd.DataFrame(np.random.rand(10, 4), columns=['a', 'b', 'c', 'd'])
    ax4.plot(df2, linestyle='--', marker='.')
    plt.savefig('./images/matplotlib04.png')
    return '/images/matplotlib04.png'

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib04.png)

    :::python
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import matplotlib
    
    df = pd.DataFrame(np.random.randn(1000, 4), columns=list('ABCD'))
    df = df.cumsum()
    
    df.plot(style='--', alpha=0.5, grid=True,
            figsize=(8, 6), subplots=True, layout=(2, 2))
    plt.subplots_adjust(wspace=0, hspace=0.2)
    plt.savefig('./images/matplotlib05.png')
    return '/images/matplotlib05.png'

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib05.png)

## Series 直接生成图表 

    :::python
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import matplotlib
    
    ts = pd.Series(np.random.randn(12),
                   index=pd.date_range('1/1/2019', periods=12))
    ts = ts.cumsum()
    
    ts.plot(
        # kind 包括，line, bar, barh
        kind='line',
        color='r',
        # linestyle -, marker . color g
        style='-gx',
        # alpha 透明度，0-1
        alpha=0.5,
        use_index=True,
        rot=0,
        ylim=[-50, 50],
        yticks=list(range(-50, 50, 10)),
        title='Time Series',
        legend=True,
        label='test')
    plt.grid(True, linestyle=':', color='gray', linewidth='0.5', axis='both')
    
    plt.savefig('./images/matplotlib06.png')
    return '/images/matplotlib06.png'

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib06.png)

## Dataframe 直接生成图表 

    :::python
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import matplotlib
    
    df = pd.DataFrame(np.random.randn(12, 4),
                      index=pd.date_range('1/1/2019', periods=12), columns=list('abcd'))
    df = df.cumsum()
    df.plot(
        style='--.',
        alpha=0.8,
        ylim=[-100, 100],
        figsize=(10, 8),
        grid=True,
        yticks=list(range(-100, 125, 25)),
        title='test',
        subplots=True)
    plt.grid(True, linestyle=':', color='gray', linewidth='0.5', axis='both')
    
    plt.savefig('./images/matplotlib07.png')
    return '/images/matplotlib07.png'

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib07.png)

## 柱关图与堆叠图 

    :::python
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import matplotlib
    
    fig, axes = plt.subplots(4,1, figsize=(12,12))
    s = pd.Series(np.random.randint(0,10,16), index=list('abcdefghijklmnop'))
    df=pd.DataFrame(np.random.rand(10,3), columns=list('ABC'))
    
    # 单系列柱状图
    s.plot(kind='bar', ax=axes[0], grid=True,legend=True,label='s',alpha=0.6)
    
    # 多系列柱状图
    df.plot(kind='bar',ax=axes[1],colormap='Reds_r')
    
    # 多系列堆叠图
    df.plot(kind='bar',ax=axes[2], colormap='Blues_r', stacked=True)
    
    df.plot.barh(ax=axes[3],grid=True,stacked=True,colormap='BuGn_r')
    
    plt.savefig('./images/matplotlib08.png')
    return '/images/matplotlib08.png'

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib08.png)

## 堆叠柱状图 

    :::python
    import numpy as np
    import matplotlib.pyplot as plt
    
    category_names = ['Strongly disagree', 'Disagree',
                      'Neither agree nor disagree', 'Agree', 'Strongly agree']
    results = {
        'Question 1': [10, 15, 17, 32, 26],
        'Question 2': [26, 22, 29, 10, 13],
        'Question 3': [35, 37, 7, 2, 19],
        'Question 4': [32, 11, 9, 15, 33],
        'Question 5': [21, 29, 5, 5, 40],
        'Question 6': [8, 19, 5, 30, 38]
    }
    
    
    def survey(results, category_names):
        labels = list(results.keys())
        data = np.array(list(results.values()))
        data_cum = data.cumsum(axis=1)
        category_colors = plt.get_cmap('RdYlGn')(
            np.linspace(0.15, 0.85, data.shape[1]))
    
        fig, ax = plt.subplots(figsize=(9.2, 5))
        ax.invert_yaxis()
        ax.xaxis.set_visible(False)
        ax.set_xlim(0, np.sum(data, axis=1).max())
    
        for i, (colname, color) in enumerate(zip(category_names, category_colors)):
            widths = data[:, i]
            starts = data_cum[:, i] - widths
            ax.barh(labels, widths, left=starts, height=0.5,
                    label=colname, color=color)
            xcenters = starts + widths / 2
    
            r, g, b, _ = color
            text_color = 'white' if r * g * b < 0.5 else 'darkgrey'
            for y, (x, c) in enumerate(zip(xcenters, widths)):
                ax.text(x, y, str(int(c)), ha='center', va='center',
                        color=text_color)
        ax.legend(ncol=len(category_names), bbox_to_anchor=(0, 1),
                  loc='lower left', fontsize='small')
    
        return fig, ax
    
    
    survey(results, category_names)
    
    plt.savefig('./images/matplotlib09.png')
    return '/images/matplotlib09.png'

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib09.png)

## 散点图 

    :::python
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import matplotlib
    data = {'a': np.arange(50),
            'c': np.random.randint(0, 50, 50),
            'd': np.random.randn(50)}
    data['b'] = data['a'] + 10 * np.random.randn(50)
    data['d'] = np.abs(data['d']) * 100
    
    plt.scatter('a', 'b', c='c', s='d', data=data)
    plt.xlabel('entry a')
    plt.ylabel('entry b')
    
    plt.savefig('./images/matplotlib010.png')
    return '/images/matplotlib010.png'

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib010.png)

## 图中插入数据表 

```python
import numpy as np
import matplotlib.pyplot as plt

data = [[66386, 174296, 75131, 577908, 32015],
        [58230, 381139, 78045, 99308, 160454],
        [89135, 80552, 152558, 497981, 603535],
        [78415, 81858, 150656, 193263, 69638],
        [139361, 331509, 343164, 781380, 52269]]

columns = ('Freeze', 'Wind', 'Flood', 'Quake', 'Hail')
rows = ['%d year' % x for x in (100, 50, 20, 10, 5)]

values = np.arange(0, 2500, 500)
value_increment = 1000
colors = plt.cm.BuPu(np.linspace(0, 0.5, len(rows)))
n_rows = len(data)

index = np.arange(len(columns)) + 0.3
bar_width = 0.4
y_offset = np.zeros(len(columns))
cell_text = []
for row in range(n_rows):
    plt.bar(index, data[row], bar_width, bottom=y_offset,
            color=colors[row], edgecolor='black')
    y_offset = y_offset+data[row]
    cell_text.append(['%1.1f' % (x/1000.0) for x in y_offset])

colors_col = plt.cm.Reds(np.linspace(0, 0.5, len(rows)))
colors = colors[::-1]
cell_text.reverse()

the_table = plt.table(cellText=cell_text,
                      rowLabels=rows,
                      rowColours=colors,
                      colLabels=columns,
                      colColours=colors_col,
                      loc='bottom')
plt.subplots_adjust(left=0.2, bottom=0.2)

plt.ylabel("Loss in ${0}'s".format(value_increment))
plt.yticks(values * value_increment, ['%d' % val for val in values])
plt.xticks([])
plt.title('Loss by Disaster')

plt.savefig('./images/matplotlib011.png')
return '/images/matplotlib011.png'
```

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib011.png)

## 面积图 

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
fig, axes = plt.subplots(2, 1, figsize=(10, 8))
df1 = pd.DataFrame(np.random.rand(10, 4), columns=list('abcd'))
df2 = pd.DataFrame(np.random.randn(10, 4), columns=list('abcd'))
df1.plot.area(colormap='Greens_r', alpha=0.8, ax=axes[0])
df2.plot.area(stacked=False, colormap='Set2', alpha=0.8, ax=axes[1])
plt.savefig('./images/matplotlib012.png')
return '/images/matplotlib012.png'
```

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib012.png)

## 3d 图例 

```python
from mpl_toolkits.mplot3d import Axes3D
import matplotlib.pyplot as plt
import numpy as np

np.random.seed(196608081)


def randrange(n, vmin, vmax):
    return (vmax-vmin)*np.random.rand(n)+vmin


fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
n = 100

for m, zlow, zhigh in [('o', -50, -25), ('^', -30, -5)]:
    xs = randrange(n, 23, 32)
    ys = randrange(n, 0,100)
    zs = randrange(n, zlow, zhigh)
    ax.scatter(xs, ys, zs, marker=m)

ax.set_xlabel('X Label')
ax.set_ylabel('Y Label')
ax.set_zlabel('Z Label')
plt.savefig('./images/matplotlib013.png')
return '/images/matplotlib013.png'
```

![](https://cdn.jsdelivr.net/gh/zucchiniy/blog-assets@master/images/matplotlib013.png)

## 更多图例 

更多内容内 [Matplotlib Gallery](https://matplotlib.org/gallery/index.html)，可以从中找到想使用的图例进行使用。
