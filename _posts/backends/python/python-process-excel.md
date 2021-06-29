---
title: 利用 Python 生成数据透视表
tags:
  - 透视表
  - read_excel()
categories:
  - 后台技术
  - Python
keywords:
  - read_excel
  - head
  - shape
  - dropna
description: 简单的利用 Python 实现 Excel 文件的处理。
abbrlink: f24bdf86
date: 2019-08-09 06:53:00
---


## 简介

- 利用 `read_excel()` 的 *usecols* 参数来指定表的某一列，以方便排除不必要的干扰列
- 养成数据加载以后，使用 `head()` 进行预览的习惯
- 养成使用 `shape()` 及 `info()` 了解表格基本情况的习惯

利用 `info()` 方法查看数据中是否有空值，如果有空值的话，则可以使用 `dropna()` 方法将其移除。

需要掌握的主要有两个方法:

- `DataFrame.insert()` 方法，用来增加对应的列
- `DataFrame.pivot_table()` 产生透视图，展示重要的数据

<!--more-->

## 具体方法

- `DataFrame.insert(self, loc, column, value, allow_duplicates=False)`

loc : int 表示第几列；0 <= loc <= len(columns)
column : string, number, or hashable object;给插入的列取名，如 column='新的一列'
value : int ，array，series
allow_duplicates : bool 是否允许列名重复，选择 True 表示允许新的列名与已存在的列名重复。

- `DataFrame.pivot_table(self, values=None, index=None, columns=None, aggfunc='mean', fill_value=None, margins=False, dropna=True, margins_name='All', observed=False)`

values : 要进行透视展示的数据
index : 需要重新进行展示成列，是原始数据中的某一个行
columns : 要重新展示为行的内容，是原来的列或者是其它的属性，可以是列表
aggfunc : 要进行统计的行，可以是 `numpy.sum` / `numpy.mean` 等，也可以按列进行统计 `aggfunc={'c1' : numpy.mean, 'c2' : numpy.sum}`
fill_value : 将缺失值替换的值，幽灵将 Nan 换成 0 : `fill_value=0`
margins : bool, 增加行或者列的汇总信息
dropna : bool ，是否要删除为空的信息
margin_name : string , 默认为 all ，或者自定义一个名称 observed bool , True 显示分类中的数据，False 显示所有数据，默认为 False

## 示例代码

```python
import pandas as pd
from datetime import datetime

data = pd.read_excel(r'python_learning.xlsx',
                     usecols=[1, 4, 6, 7, 8, 9, 10, 11, 12], sheet_name='sheetName')
data = data[data['合同生效日'] > datetime(2018, 12, 31)]

# 按逻辑，将一组数据拆成三组
data1 = data[["used", "loan amount", "company1", "percent1"]]
data2 = data[["used", "loan amount", "company2", "percent2"]]
data3 = data[["used", "loan amount", "company3", "percent3"]]

# 将三组内容，重新命名之后合成一个新表
data1 = data1.rename(columns={"company1": "company", "percent1": "percent"})
data2 = data2.rename(columns={"company2": "company", "percent2": "percent"})
data3 = data3.rename(columns={"company3": "company", "percent3": "percent"})

data4 = pd.concat([data1, data2, data3], ignore_index=True)

# 将数据中的空值清除
data4 = data4.dropna()

# 插入新的数据
# 1. insert() 方法
data4.insert(2, "devide percent", data4["percent"]/100)
data4.insert(5, "devide amount", data4["loan amount"]*data4["deivide percent"]/10000, False)

# 普通索引方式插入
# data4["loan divide amount"] = data4["load amount"]*data4["deivide percent"]/10000

# 增加数据透视
data5 = data4[['company', 'used', 'loan amount']]
data6 = pd.pivot_table(data5, values="loan divide amount", columns="used", index="company",
                       aggfunc='sum', fill_value=0, observed=False).reset_index()
print(data6.head())
```
