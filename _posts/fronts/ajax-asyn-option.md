---
title: Ajax 关闭异步请求
tags:
  - async
  - 异步请求
categories:
  - 前端技术
  - ajax
keywords:
  - ajax
  - async
description: 停止 ajax 的异步请求，使用同步请求进行后台访问。
abbrlink: a97db946
date: 2018-06-20 07:21:00
---

在代码中，因为进行了后台的取值操作，导致有些内容还未加载就执行到了新的地方，所以想着 ajax 的异步关闭来解决。

`async` 设置为 `false` 的时候，变成同步操作，默认( `true` )为异步操作。

```javascript
$.ajax({
cache: false,
async: false,   // 太关键了，学习了，同步和异步的参数
});
alert("2");
```
