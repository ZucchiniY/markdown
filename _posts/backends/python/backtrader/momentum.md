---
title: 动量策略
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
  - 动量策略回测
description: 构建动量策略，看看与富国天惠差多少。
headimg: 'https://cdn.jsdelivr.net/gh/zucchiniy/common@master/cover/quant.jpg'
thumbnail: 'https://cdn.jsdelivr.net/gh/zucchiniy/common@master/cover/quant.jpg'
abbrlink: 28ed95b9
date: 2021-07-12 21:38:22
updated: 2021-07-13 21:38:22
---

## 理论

动量策略： **股票的收益率有延续原来的运动方向的趋势。——强者恒强。**

依据这个理论，还有一个延伸的出来的策略——动量反转策略： **表现差的股票在其后的一段时间内有强烈的趋势经历相当大的逆转。——弱者转强。**

## 策略

依据这两个理论反做的交易策略，也就是在一段时间内，选择效果最好的或者最差的期待配置。

## 回测

### 动量策略

选择一个基金组合，从中选择动量最好的一支基金买入持有至下一个周期。

```python momentum
def next(self):
    buy_id = 999
    c = [
        i.momentum[0]  #/ (i.datas[0].close + i.datas[0].open) * 2
        for i in self.mom
    ]
    index, value = c.index(max(c)), max(c)
    if value > 0:
        buy_id = index

    for i in range(0, len(c)):
        if i != buy_id:
            position_size = self.broker.getposition(
                data=self.datas[i]).size
            if position_size != 0:
                self.order_target_percent(data=self.datas[i], target=0)

    if buy_id != 999:
        position_size = self.broker.getposition(
            data=self.datas[buy_id]).size
        if position_size == 0:
            self.order_target_percent(data=self.datas[buy_id], target=0.98)
```

实现的思路非常简单，就是计算出基金组合中所有的基金的动量值，然后取最高的一支买入，然后每天都依据收盘价计算出动量最好的一支基金，如果不再是最好的，则卖出当前的基金，买入最好的那支。

基金组合选择的是 **沪深 300**，**中证 500** 和 **创业板** 三支。

回测最近三年的数据，起始资金为 140000.00 元。与上次的数据做一下比较，可以说是非常好的，回撤也才 22% 。

| 总利润    | 投资回报率 | 夏普指数 | 最大回撤 |
|-----------|------------|----------|----------|
| 127138.56 | 190.81%    | 1.12     | 12.71%   |

生成了一张策略的交易图，看了一下其中的买卖点。

![动量策略买卖点](https://cdn.jsdelivr.net/gh/zucchiniy/common@master/images/momentum.png)

## 优化思路

### 动量反转

动量策略中，取绝对值，如果最低的那支为负且绝对值大于动量最高值，则买入反转基金。

## 总结

策略的收益还是不错的，从 2015 年开始到现在，约 128% 的收益，与富国天惠比较一下，将时间拉到 2005 年 11 月 16 日。

再重新跑一下。

| 基金         | 投资回报率 | 夏普指数 | 最大回撤 |
|--------------|------------|----------|----------|
| 动量策略     | 4.36       | 0.62     | 22.05    |
| 富国天惠     | 21.55      | 1.04     | 28.37    |

恩，还是比富国天惠差很多呀。

