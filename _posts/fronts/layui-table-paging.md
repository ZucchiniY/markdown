---
title: Layui 表格
tags:
  - layui
categories:
  - 前端技术
  - JavaScript
description: Layui 中实现表格分页与复选框。
abbrlink: 91a72e5d
date: 2018-08-02 06:35:00
---

**Layui** 分页是由 **laypage** 实现的，所以既需要分页 **laypage** 还需要数据表格相关的内容。

## 数据表格设置 

```javascript
table.render({
page: true
...
})
```

这样就可以进行分页了，但是如果想要修改分页的样式，可以按下面的方式进行修改：

```javascript
table.render({
    page: {
        layout: ['limit','count','prev','page','next','skip'] // 分页布局
        ,groups: 1 // 只显示1个连续页码
        ,first: false // 不显示首页
        ,last: false // 不显示尾页
        ,theme: '#c00' // 可以传入颜色或者任意普通字符
    }
})
```

其中 _layout_ 中支持数据有：

1.  _count_ 总条目输区域
2.  _prev_ 上一页区域
3.  _page_ 分页区域
4.  _next_ 下一页区域
5.  _limit_ 条目选项区域
6.  _refresh_ 页面刷新区域
7.  _skip_ 快捷跳页区域


## ~实现复选框~

在后来的版本，已经提供了复选框功能了。

## 实现 

首先要在 **templet** 加上行号数据

```html
<script type="text/html" id="test_id">
  <input type="checkbox" title="testbox" id="id{{d.LAY_TABLE_INDEX}}">
</script>
```

然后在 **table** 的表头中增加对应的内容

```javascript
table.render({
    cols: [[ //表头
        field: 'test'
        ,title: 'test'
        ,templet: '#test_id'
    ]]
});
```

然后对数据进行赋值

```javascript
var data = res.data;
for(var item in data){
    $('#testbox' + data[item].LAY_TABLE_INDEX).attr('checked', 'checked');
}
form.render();
```

这主要是因为 **table** 中默认有一个数值，叫 **LAY_TABLE_INDEX** 是用来做为返回的组数据的行号，这里使用这个参数对每一行进行操作，就可以了。
