---
title: 动量摆动策略
icons:
  - fas fa-fire red
  - fas fa-star green
tags:
  - 动量效应
  - Momentum
categories:
  - 后台技术
  - Python
  - backtrader
keywords:
  - 动量摆动策略回测
description: 构建动量摆动策略，看看与富国天惠差多少。
headimg: 'https://cdn.jsdelivr.net/gh/zucchiniy/common@master/cover/quant.jpg'
thumbnail: 'https://cdn.jsdelivr.net/gh/zucchiniy/common@master/cover/quant.jpg'
abbrlink: 28ed95b9
date: 2021-07-28 13:38:22
updated: 2021-07-28 13:38:22
---

## 策略

动量策略中，只计算了差值，但实际上有一些上涨可能因为本身价格过高导致无法准备的表达具体的收益变化，所以使用百分比来表示上涨的量。

## 回测

基金组合选择的是 **沪深 300**，**中证 500** 和 **创业板** 三支。

回测最近三年的数据，起始资金为 1000000.00 元。与上次的数据做一下比较，可以说是非常好的，回撤也才 22% 。

| 投资回报率 | 夏普指数 | 最大回撤 |
|------------|----------|----------|
| 2.31       | 1.64     | 18.95%   |

生成了一张策略的交易图，看了一下其中的买卖点。

![动量策略买卖点](https://cdn.jsdelivr.net/gh/zucchiniy/common@master/images/momentum.png)

## 优化思路

### 动量反转

动量策略中，取绝对值，如果最低的那支为负且绝对值大于动量最高值，则买入反转基金。

经过回撤，反转策略并不是特别明显，一直是亏损，可能是组合只有三支的原因。

## 总结

因为创业板的时候比较短，所以只回看一下近 5 年的变化。


| 基金     | 投资回报率 | 夏普指数 | 最大回撤 |
|----------|------------|----------|----------|
| 动量摆动 | 1.03       | 0.57     | 32.45    |
| 富国天惠 | 1.30       | 1.04     | 28.37    |

恩，还是比富国天惠差很多呀。
