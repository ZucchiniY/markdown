---
title: 股票数据库构建
tags:
  - 股票数据
categories:
  - 后台技术
  - Python
  - backtrader
description: 创建一个股票数据库并定期更新数据。
abbrlink: ed8ff4e7
date: 2020-10-08 02:06:00
updated: 2021-07-08 14:20:38
---

## 背景

有一段时间沉迷于股票，就希望能通过代码把股票的行情都给抓下来，存到数据库中，但是这本身是一个非常大的工程，而且数据文件非常大。

下面是构建股票数据库和利用 Tushare 进行数据获取的方案。但是后面就放弃了，改使用 Akshare 进行数据获取了。

再后面放弃了股票的相关内容，改为买卖场内基金获利。

## 构建本地数据库

```sql
* 沪深所有股票
 * 表中的字段对应tushare的stock_basic接口返回字段，可以查看此接口对应的字段解释
  */
CREATE TABLE `stocks`  (
  `id` int(10) UNSIGNED NOT NULL AUTO_INCREMENT,
  `ts_code` varchar(16) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `symbol` varchar(16) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NULL DEFAULT NULL,
  `name` varchar(16) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NULL DEFAULT NULL,
  `area` varchar(16) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NULL DEFAULT NULL,
  `industry` varchar(16) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NULL DEFAULT NULL,
  `fullname` varchar(64) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NULL DEFAULT NULL,
  `market` varchar(16) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NULL DEFAULT NULL,
  `list_status` varchar(2) CHARACTER SET utf8 COLLATE utf8_unicode_ci NULL DEFAULT NULL,
  `list_date` timestamp(0) NULL DEFAULT NULL,
  `delist_date` timestamp(0) NULL DEFAULT NULL,
  `is_hs` varchar(2) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE INDEX `idx_stocks_ts_code`(`ts_code`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 3836 CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

/*
 * 沪深所有股票日交易数据
 * 表中的字段解释对应tushare的daily接口返回字段，可以查看此接口对应的字段解释
*/
CREATE TABLE `trade_data`  (
  `id` bigint(20) UNSIGNED NOT NULL AUTO_INCREMENT,
  `ts_code` varchar(16) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `trade_date` timestamp(0) NULL DEFAULT NULL,
  `open` float NOT NULL DEFAULT 0,
  `high` float NOT NULL DEFAULT 0,
  `low` float NOT NULL,
  `close` float NOT NULL,
  `pre_close` float NOT NULL,
  `change` float NOT NULL,
  `pct_chg` float NOT NULL,
  `vol` float NOT NULL,
  `amount` float NOT NULL DEFAULT 0,
  `cdate` timestamp(0) NULL DEFAULT CURRENT_TIMESTAMP(0) ON UPDATE CURRENT_TIMESTAMP(0),
  PRIMARY KEY (`id`) USING BTREE,
  UNIQUE INDEX `idx_trade_code_date`(`ts_code`, `trade_date`) USING BTREE,
  INDEX `idx_date_pct_chg`(`trade_date`, `pct_chg`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 7187583 CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '2002年至今日交易数据' ROW_FORMAT = Dynamic;
```

## 数据下载

下载沪深所有股票基础信息

``` python
data = pro.stock_basic(fields='ts_code,symbol,name,fullname,area,industry,market,list_status,list_date,delist_date,is_hs')
pd.io.sql.to_sql(data, 'stocks', con=engine,index=False, if_exists='append',chunksize=500)
```

更新日交易数据

``` python
# 批量更新日交易数据
def update_trade(self,beg_id,end_id,start_date,end_date):
    session = DBSession()
    data = session.query(dbStock).filter(dbStock.id < end_id+1, dbStock.id > beg_id).all()
    for dt in data:
        daily = pro.daily(ts_code= dt.ts_code, start_date=start_date, end_date=end_date)
        print('stock:%s,count:%s' % (dt.id,daily.shape[0]))
        pd.io.sql.to_sql(daily, 'trade_data', con=engine, index=False, if_exists='append', chunksize=500)
    print('finshed')

# 调用方法
id_ticket = 0 
# id从1-50的股票，获取从20020418到2020601的交易数据
update_trade(id_ticket, id_ticket + 50, '20020418', '2020812')
```

## 数据同步

```python
# 每天更新股票交易数据
def update_trade():
    if is_holiday():
        return False
    day = time.strftime('%Y%m%d', time.localtime())
    daily = pro.daily(trade_date=day)
    pd.io.sql.to_sql(daily, 'trade_data', con=engine, index=False, if_exists='append', chunksize=500)
    print('%s更新了%s条记录' % (day,len(daily)))

# 定时任务设定每晚8:30更新
schedule.every().day.at('20:30').do(on_timer, 'update_trade')
```
