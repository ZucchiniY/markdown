---
title: Python 学习路径
author: zucchini
tags:
  - Python
categories:
  - 生活总结
description: 按照自己的理解，对 Python 的水平做了一个分组，算了一下，自己原来是初级。
abbrlink: df5adb66
date: 2019-08-07 06:54:00
---

现在 Python 主要在 **前端** 、 **数据分析** 两个方面比较火，相较于其它语言，更灵活，经过一段时间的选择之后，希望可以认真的学习 Python 这门编程语言。

## Python 的级别 

对于我们这些程序员来说，总要有一个级别，不然怎么能知道自己在哪个级别呢？


### 一级——了解基本语法

-   [X] 掌握了基本的语法，可以通过 Python 实现常用的需求。不管代码质量怎么样。
-   [ ] [The Python Tutorial 3.8](https://docs.python.org/3.8/tutorial/index.html)

### 二级——熟练使用常用的库

-   [ ] 熟悉常用的 Standard 库的使用。
-   [ ] [The Python Standard Library](https://docs.python.org/3.8/library/index.html)
-   [ ] 熟悉常用的第三方库，要看各自领域中的内容，例如 pandas、flask 等

#### Pythonic 的小技能

- [ ] 善用内置函数
    - [ ] map
    - [ ] zip
    - [ ] enumerate
    - [ ] reversed
    - [ ] any all
- [ ] 小细节
    - [ ] raise SystemExit
    - [ ] 文件的 x 模式
    - [ ] ConfigParser
    - [ ] defaultdict
    - [ ] Counter
    - [ ] nametuple
- [ ] 使用高级并发工具
- [ ] 使用装饰器
- [ ] 使用设计模式
- [ ] 全局变量
- [ ] 时间复杂度
- [ ] 上下文管理器
  - [ ] 管理锁
  - [ ] 管理数据库 cursor
  - [ ] 运算精度
  - [ ] 同时管理多个资源
  - [ ] 实现上下文管理协议


### 三级——Pythonic

让编码更优雅，更符合 Python 也就是 Pythonic 而不是用 Python 写 Java 类型的代码，比如 with、for-else、try-else、while-else、yield 等。

另外还需要掌握一些实现原理，了解 Python 在语法层面的一些协方，可以自己实现语法糖。比如（上下文管理器）等。

-   [ ] [The PythonLanguage Reference](https://docs.python.org/3.8/reference/index.html)

-   [ ] [Python HOWTOs](https://docs.python.org/3.8/howto/index.html)


### 四级——高级玩法

-   [ ] 掌握 Python 的内存机制、GIL限制等
-   [ ] 知道如何改变 Python 的行为
-   [ ] 可以轻松写出高质量的 Python 代码
-   [ ] 能够轻松分辨不同的 Python 代码效率并知道如何优化


### 五级——看透本质

-   [ ] 阅读 Python 的 C 实现
-   [ ] 掌握 Python 中各种对象的本质，掌握是如何通过 C
-   [ ] 实现对象行为，对于常见的数据结构，掌握其实现细节
