---
title: 策略收益比较
icons:
  - fas fa-fire red
  - fas fa-star green
tags:
  - 投资策略
categories:
  - 后台技术
  - Python
  - backtrader
keywords:
  - 基金投资
  - 策略比较
pin: true
description: 设计自己的投资策略，逐步增加总收益。
headimg: 'https://cdn.jsdelivr.net/gh/zucchiniy/common@master/cover/quant.jpg'
thumbnail: 'https://cdn.jsdelivr.net/gh/zucchiniy/common@master/cover/quant.jpg'
abbrlink: bcb5a920
date: 2021-07-13 16:37:35
updated: 2021-07-13 16:37:35
---

## 标杆基金

标杆基金有几支，其中最强的就是富国天惠成长混合，最近比几年比较火的兴全和润分级混合还有我比较关注的广发多因子混合。

## 投资策略

目前已经进入实盘的有两个策略，分别是网格策略和布林线策略。其它的主要是一些回测数据的比较，后面会逐渐把这些策略进行实盘的。

**因为一般比较关注的是近三年的收益情况，所以回没主要以近三年的数据进行比较。**

### 周内策略

{% post_link backends/python/backtrader/week-effect 周内交易策略 %}

### 定投策略

### 动量策略

{% post_link backends/python/backtrader/momentum-osciliator 动量策略 %}

### 动量钟摆策略

{% post_link backends/python/backtrader/momentum-osciliator 动量钟摆策略 %}

## 收益比较

| 基金                    | 近三年收益 | 近三年最大回撤 | 夏普比率 |
|:------------------------|-----------:|:--------------:|:--------:|
| 富国天惠成长混合 161005 |    102.96% | 28.37%         | 1.04     |
| 兴全合润混合 163406     |    153.36% | 19.98%         | 1.44     |
| 广发多因子混合 002943   |    203.57% | 23.67%         | 1.56     |
| 周内交易策略            |     154.4% | 17.65%         | 0.64     |
| 动量策略                |    184.14% | 12.71          | 1.02     |
| 动量钟摆策略 2.0        |    188.35% | 17.09          | 1.21     |


