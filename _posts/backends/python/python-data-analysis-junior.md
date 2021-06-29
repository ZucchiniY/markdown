---
title: Python 数据分析初阶
tags:
  - pandas
categories:
  - 后台技术
  - Python
description: Python 数据分析入门笔记。
abbrlink: a3644317
date: 2019-10-18 02:19:00
---


## 某一列数据计算

```python
data['column_name'].value_counts()
```


以之前找到的一个前辈的数据为例子，首先我们要获取文件

```python
import pandas as pd
data = pd.read_excel('xxxx.xls')
```


这里可以单独查看其中的内容 `data['nick']`，计算其中的大小则使用 `data['nick'].value_counts()`。

同样的情况，我们可以增加分组并获取对应的数据

```python
data1 = data['score'].groupby(data['city']) data1.mean()
```


这种情况下可以类比为SQL语句：

```sql
select avg(score) from data group by city
```


这样的数据看起来不是特别让人喜欢，这个时间我们可以给他排个序：

```python
data1.mean().sort_values(ascending=False)
```


现在看起来好多了，但是有点多了，我们只想看前几条记录：

```python
data1.mean().sort_values(ascending=False).head(3)
```


可惜了，好多城市我都没听过，我只想看直辖市的数据

```python
data2 = data.loc[(data['city'].isin(['北京','天津','重庆','上海']))]
```


但是这样还是不特别好看，我们可以再按城市看一下，评分有多少

```python
data2['score'].groupby(data2['city']).mean()
```


## 数据表信息查看 

`df.shape`: 维度查看
`df.info()`: 数据表基本信息，包括围度、列名、数据格式、所占空间
`df.dtypes`: 每一列的数据格式
`df['b'].dtype`: 某一列的格式
`df.isnull()`: 是否空值
`df.['b'].unique()`: 查看某一列的唯一值
`df.values`: 查看数据表的值
`df.columns`: 查看列名
`df.head()`: 查看默认的前 10 行数据
`df.tail()`: 查看默认的后 10 行数据


## 数据表清洗

`df.fillna(value=0)`: 用数字 0 填充空值 
`df['pr'].fillna(df['pr'].mean())`: 用列 pr 的平均值对 na 进行填充
`df['city']=df['city'].map(str.strip)`: 清除 city 字段的字符空格
`df['city']=df['city'].str.lower()`: 大小写转换
`df['pr'].astype('int')`: 更改数据的格式
`df.rename(columns={'category': 'category-size'})`: 更改列名
`df['city'].drop_duplicates()`: 删除后出现的重复值
`df['city'].drop_duplicates(keep='last')`: 删除先出现的重复值
`df['city'].replace('sh', 'shanghai')`: 数据替换


## 数据预处理 

-   数据表合并

```python
df_inner = pd.merge(df, df1, how='inner')  # 匹配合并，交集
df_left = pd.merge(df, df1, how='left')  # 左联表
df_right = pd.merge(df, df1, how='right')  # 右联表
df_outer = pd.merge(df, df1, how='outer')  # 并集
```


-   设置索引列

```python
df.set_index('id')
```


-   按照特定列的值排序

```python
df.sort_values(by=['age'])
```


-   按照索引列排序

```python
df.sort_index()
```


-   如果 pr 列的值大于 3000 ， group 列显示 hight , 否则显示 low

```python
df['group'] = np.where(df['pr'] > 3000, 'hight', 'low')
```


-   对复合多个条件的数据进行分级标记

```python
df.loc[(df['city'] == 'beijing') & (df['pr'] >= 4000), 'sign'] = 1
```


-   对 category 字段的值依次进行分列，并创建数据表，索引值 df 的索引列，列名称为 category 和 size

```python
pd.DataFrame((x.split('-') for x in df['category']), index=df.index, columns=['category', 'size'])
```
        

## 数据提取 

`loc`: 函数按标签值进行提取
`iloc`: 按位置进行提取
`ix`: 可以同时按标签和位置进行提取

具体的使用见下：

`df.loc[3]`: 按索引提取单行的数值
`df.iloc[0:5]`: 按索引提取区域行数据值
`df.reset_index()`: 重设索引
`df=df.set_index('date')`: 设置 date 为索引
`df[:'2013']`: 提取 2013 之前的所有数据
`df.iloc[:3,:2]`: 从 0 位置开始，前三行，前两列，这里的数据不同去是索引的标签名称，而是数据所有的位置
`df.iloc[[0,2,5],[4,5]]`: 提取第 0、2、5 行，第 4、5 列的数据
`df.ix[:'2013',:4]`: 提取 2013 之前，前四列数据
`df['city'].isin(['beijing'])`: 判断 city 的值是否为北京
`df.loc[df['city'].isin(['beijing','shanghai'])]`: 判断 city 列里是否包含 beijing 和 shanghai ，然后将符合条件的数据提取出来
`pd.DataFrame(category.str[:3])`: 提取前三个字符，并生成数据表


## 数据筛选 

使用与、或、非三个条件配合大于、小于、等于对数据进行筛选，并进行计数和求和。

-   使用与进行筛选

```python
df.loc[(df['age'] > 25) & (df['city'] == 'beijing'), ['id', 'city', 'age', 'category']]
```


-   使用或进行筛选

```python
df.loc[(df['age'] > 25) | (df['city'] == 'beijing'), ['id', 'city', 'age']]
```


-   使用非进行筛选

```python
df.loc[(df['city'] != 'beijing'), ['id', 'city', 'age']].sort(['id'])
```


-   筛选后的灵气按 city 列进行计数

```python
df.loc[(df['city'] != 'beijing'), ['id', 'city', 'age']].sort(['id']).city.count()
```


-   使用 query 函数进行筛选

```python
df.query('city' == ['beijing', 'shanghai'])
```


-   对筛选后的结果按 pr 进行求和

```python
df.query('city' == ['beijing', 'shanghai']).pr.sum()
```



## 数据汇总 

主要使用 groupby 和 pivote_table 进行处理。

`df.groupby('city').count()`: 按 city 列分组后进行数据汇总
`df.groupby('city')['id'].count()`: 按 city 进行分组，然后汇总 id 列的数据
`df.groupby(['city','size'])['id'].count()`: 对两个字段进行分组汇总，然后进行计算
`df.groupby('city')['pr'].agg([len, np.sum,np.mean])`: 对 city 进行分组，然后计算 pr 列的大小、总和和平均数


## 数据统计 

数据采样，计算标准差、协方差和相关系数。

-   简单数据采样

```python
df.sample(n=3)
```


-   手动设置采样权重

```python
weights = [0, 0, 0, 0, 0, 0.5, 0.5]
df.sample(n=2, weights=weights)
```

-   采样后不放回

```python
df.sample(n=6, replace=False) # 如果 replace = True 采样后放回
```


-   数据表描述性统计

```python
df.describe().round(2).T  # round 表示显示的小数位数，T 表示转置
```


-   计算列的标准差

```python
df['pr'].std()
```


-   计算两个字段间的协方差

```python
df['pr'].cov(df['m-point'])
```


-   计算表中所有字段间的协方差

```python
df.cov()
```


-   两个字段间的相关性分析

```python
df['pr'].corr(df['m-point'])  # 相关系数在 [-1, 1] 之间，接近 -1 为负相关，1 为正相关，0 为不相关
```

-   数据表的相关性分析

```python
df.corr()
```

