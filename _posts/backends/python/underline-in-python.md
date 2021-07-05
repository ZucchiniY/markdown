---
title: _，__，__xx__ 的区别
tags:
  - 下划线
  - 双下划线
  - 魔法方法
categories:
  - 后台技术
  - Python
keywords:
  - _
  - __
  - __xx__
  - magic mathod
description: Python 中的私有方法、静态方法和魔法方法。
abbrlink: b44fc2d6
date: 2021-07-05 09:38:00
updated: 2021-07-05 09:38:00
---

## 单下划线开头 —— Python 中的私有属性或方法

在 Python 中并没有真正的私有属性或方法，在 PEP8 中，使用前置单下划线的方式来定义私有成员变量和方法，而在使用通配符从模块中导入的时候，是看不到单下划线开头的属性的。

但是这个方法在内部使用的时候是没有问题的。


## 单下划线结尾 —— Python 中关键字冲突

有时候一个变量最适合的名称被一个关键字所占用，可以使用单下划线结尾的方式来结尾，比较常见的是像 `class` 和 `def` 这种关键字，而我们可以通过后缀一个下划线的方式来使用 ，有点类似 Java 中的 `clazz` 的命名，中不过这里用的是 `class_` 。

这个方法也是在 PEP8 中引入了对应的解释，类似大家都这样用的约定性的定义。

## 双下划线开头 —— Python 中可被重写的属性名

有些文章中表示 “双下划线开头的方法是私有变量” 是错误的，而且还举例子说你在调用对应的方法的时候是不能调用双下划线方法的，但是这种理解是错误的，可以使用下面的方法做一下测试：

![Python 图](https://cdn.jsdelivr.net/gh/zucchiniy/common@master/images/underline-in-python-00.png)

在图中可以看到，调用报错是因为方法中关没有 `__name` 和 `__get_name` 两个方法，到而代之的是 `_Person_get_name` ，所以这才是导致上面的调用报错的原因，而不是因为这是私有方法。

[PEP8](https://www.python.org/dev/peps/pep-0008/#id47)

原文是这样写的 *Use one leading underscore only for non-public methods and instance variables.* 

而在这里的双下划线叫作 “名称修饰(name mangling)” 也就是解释器会修改变量的名称，以防止类被扩展的时候产生冲突。所以在编译之后，会在双下划线变量或方法前加上类的名称。

## 双下划线开头且双下划线结尾 —— Python 中的魔法方法

这个其实是 Python 中最常见的方法，最常见的就是 `__init__` 和 `__name__` ，这里就不再赘述了。


## 单独的下划线 —— Python 中无关紧要的变量

这个其实也是非常常见的一个使用方法，最常见的是在循环中，不需要使用循环变量的时候使用。

```python
for _ in range(10):
    print(test1)
```

## PEP8 原文

[PEP8](https://www.python.org/dev/peps/pep-0008/#id34)

- _single_leading_underscore: weak "internal use" indicator. E.g. from M import * does not import objects whose names start with an underscore.

- single_trailing_underscore_: used by convention to avoid conflicts with Python keyword, e.g.

```python
tkinter.Toplevel(master, class_='ClassName')
```

- __double_leading_underscore: when naming a class attribute, invokes name mangling (inside class FooBar, __boo becomes _FooBar__boo; see below).

- __double_leading_and_trailing_underscore__: "magic" objects or attributes that live in user-controlled namespaces. E.g. __init__, __import__ or __file__. Never invent such names; only use them as documented.


