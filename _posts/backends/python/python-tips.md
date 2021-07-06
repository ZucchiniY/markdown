---
title: Python 小技巧
icons:
  - fas fa-fire red
  - fas fa-star green
abbrlink: 281431d
date: 2019-02-25 10:54:46
updated: 2021-07-06 10:54:46
tags:
  - Python
  - dict
  - 
categories:
  - 后台技术
  - Python
keywords:
  - Python 
  - Dict
description: 使用 Python 过程中的一些小技巧。
---

## 字典替代 if else 语句

因为 Python 中是没有 switch-case 的，所以在多个条件选择的时候，只能进行多次 if...else 方法的组合才能实现此类方法。这里可以使用字典来完成类似的功能。

``` python if...else...
def fun(x):
    if x == 'a':
        return 1
    elif x == 'b':
        return 2:
    else:
        return None
```

这样的实现方式对 Python 来说显得十分繁杂，虽然功能没有任何问题，但是不够 *Pythonic* ，而相对的，使用字典可以完成同样的效果。

```python dict
def fun(x):
    return {'a': 1, 'b':2}.get(x, None)
```

这样看起来是不是非常简练了。

## 列表中出现最多的元素

我们经常会在一组数据中找出 **出现次数最多的元素** 这个操作，一般情况下我们都是利用一个计数器来实现，如果找到数据就 *+1* 这样。

在 Python 中有两个简单的方式可以实现。

```python max()
nums = [1,2,3,4,5,3,2,4,5,3]

max(set(nums), key=nums.count)
```

这个操作是利用 max 的 key 关键字来实现的，key 一般为一个函数，用来指定取最大值的方法，这里用的是 list.count 。

另一个方法是使用 Counter 方法。

```python Counter
from collections import Counter
Counter(nums).most_common()[0][0]
```

这里 `Counter` 方法返回的是所有数据的统计，然后按从大到小排列，所以 [0,0] 就是最多的数据了。

![Python tips](https://cdn.jsdelivr.net/gh/zucchiniy/common@master/images/python-tips-00.jpg)
